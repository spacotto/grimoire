# Circular Dependencies

This document covers circular dependencies in Python 3 — what they are, why they cause problems, how to detect them, and the risks they introduce.

## What are Circular Dependencies?

A **circular dependency** occurs when two or more modules depend on each other, directly or indirectly.

```
module_a.py  →  imports module_b
module_b.py  →  imports module_a
```

This creates a closed loop that Python's import system cannot resolve cleanly.

## The Circular Import Problem

Python executes a module's code **top to bottom** the first time it is imported. If `module_a` imports `module_b`, and `module_b` tries to import `module_a` before `module_a` has finished loading, Python returns an **incomplete module object** — one that may be missing names defined later in the file.

```python
# module_a.py
from module_b import greet      # triggers module_b to load

def name():
    return "Alice"
```

```python
# module_b.py
from module_a import name       # module_a is not fully loaded yet!

def greet():
    return f"Hello, {name()}"
```

Running either file raises:

```
ImportError: cannot import name 'name' from partially initialized module 'module_a'
```

## Why Circular Imports Fail

Python tracks imports in `sys.modules`. When a module starts loading, it is registered immediately — but its body has not finished executing yet.

Steps during a circular import:

1. `module_a` starts loading → added to `sys.modules` as a partial object
2. `module_a` hits `import module_b` → Python starts loading `module_b`
3. `module_b` hits `import module_a` → Python finds it in `sys.modules`
4. Python returns the **partial** `module_a` object
5. `module_b` tries to access a name that does not exist yet → `ImportError`

The root cause is **using a name before it is defined**, because the importing module ran before the exporting module finished.

## Detecting Circular Dependencies

### At runtime

Python raises one of these errors when a circular import is triggered:

```
ImportError: cannot import name 'X' from partially initialized module 'Y'
ImportError: cannot import name 'X' from 'Y' (most likely due to a circular import)
```

### With static analysis tools

```bash
# pydeps — generates a visual dependency graph
pip install pydeps
pydeps your_package/

# pylint — reports cyclic imports
pip install pylint
pylint --disable=all --enable=cyclic-import your_package/

# importlab — traces static import chains
pip install importlab
importlab your_package/
```

### Manual inspection

Trace the import chain by reading the top-level `import` and `from ... import` statements of each module. If following the chain leads back to the starting module, a cycle exists.

## Circular Dependency Patterns

### Direct cycle (A → B → A)
```
module_a  ←→  module_b
```

### Indirect cycle (A → B → C → A)
```
module_a  →  module_b  →  module_c  →  module_a
```

### Class-level cycle
```python
# models/user.py
from models.order import Order

class User:
    def get_orders(self) -> list[Order]: ...
```

```python
# models/order.py
from models.user import User

class Order:
    def get_buyer(self) -> User: ...
```

Both modules need each other at **import time**, before either class is fully defined.

### Function-level cycle (deferred — sometimes safe)
```python
# module_a.py
import module_b

def run():
    module_b.helper()       # import already resolved by the time run() is called
```

If the circular import is deferred inside a function body rather than at the top level, Python may resolve it without error — because the import executes only when the function is called, not when the module loads.

## The Danger of Circular Imports

| Risk | Description |
|---|---|
| `ImportError` at startup | Module names missing from partially initialized objects |
| Silent partial state | A name exists but holds `None` or a stub instead of the real object |
| Unpredictable behavior | Outcome depends on which module is imported first |
| Test fragility | Test import order differs from production, masking or exposing bugs |
| Refactoring difficulty | Tightly coupled modules are hard to split, move, or reuse |
| Maintainability | Cycles signal a design problem — unclear ownership of responsibilities |

Circular imports are often a symptom of **poor separation of concerns**. The standard fix is to extract shared code into a third module that neither of the original modules imports.
