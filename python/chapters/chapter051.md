# Python Packages

As projects grow, keeping all code in a single file becomes unmanageable. Python **packages** solve this by letting you group related modules into directories — giving your code a clear, scalable structure. This document covers what packages are, how they work, and how to build them well.

## What are Packages?

A **package** is a directory that contains Python modules and a special `__init__.py` file. It lets you organise related code under a common namespace and import it cleanly.

```python
import mypackage.utils          # import a module from a package
from mypackage import helpers   # import a specific module
from mypackage.utils import fmt # import a specific function
```

>[!NOTE]
>Packages are just directories — Python recognises them as packages because of the `__init__.py` file inside.

## Package Directories

A minimal package looks like this:

```
mypackage/
├── __init__.py
├── module_a.py
└── module_b.py
```

- `mypackage/` — the package directory (its name becomes the namespace)
- `__init__.py` — marks the directory as a package
- `module_a.py`, `module_b.py` — regular Python modules inside the package

>[!TIP]
>Place the package directory in your project root or anywhere on `sys.path` so Python can find it.

## The `__init__.py` File

`__init__.py` is what turns a plain directory into a Python package. Without it, Python won't recognise the folder as importable.

It can be **empty** — that's perfectly valid:

```python
# __init__.py
# (empty)
```

Or it can contain code that runs when the package is first imported.

## Package Initialisation

When you run `import mypackage`, Python executes `mypackage/__init__.py`. You can use this to:

**Export a public API** — expose selected names at the package level:

```python
# mypackage/__init__.py
from .module_a import ClassA
from .module_b import helper_fn
```

>[!NOTE]
>Now callers can write `from mypackage import ClassA` instead of `from mypackage.module_a import ClassA`.

**Run setup code** — configure logging, set constants, or validate the environment:

```python
# mypackage/__init__.py
import logging

logging.getLogger(__name__).addHandler(logging.NullHandler())

VERSION = "1.0.0"
```

>[!TIP]
>Keep `__init__.py` light. Heavy logic or slow imports here slow down every consumer of your package.

## Creating Your First Package

**Step 1 — Create the directory structure:**

```
project/
├── main.py
└── greetings/
    ├── __init__.py
    └── messages.py
```

**Step 2 — Write a module inside the package:**

```python
# greetings/messages.py

def hello(name: str) -> str:
    return f"Hello, {name}!"

def goodbye(name: str) -> str:
    return f"Goodbye, {name}!"
```

**Step 3 — Optionally expose it via `__init__.py`:**

```python
# greetings/__init__.py
from .messages import hello, goodbye
```

**Step 4 — Import and use it:**

```python
# main.py
from greetings import hello, goodbye

print(hello("Silvia"))    # Hello, Silvia!
print(goodbye("Silvia"))  # Goodbye, Silvia!
```

>[!NOTE]
>The leading dot in `.messages` is a **relative import** — it tells Python to look inside the current package rather than searching `sys.path`.

## Nested Packages (Subpackages)

Packages can contain other packages. Each subdirectory needs its own `__init__.py`.

```
mypackage/
├── __init__.py
├── core/
│   ├── __init__.py
│   ├── parser.py
│   └── validator.py
└── utils/
    ├── __init__.py
    ├── formatting.py
    └── logging.py
```

Importing from subpackages:

```python
from mypackage.core import parser
from mypackage.utils.formatting import fmt_date

# or using relative imports inside the package:
# mypackage/core/validator.py
from ..utils.formatting import fmt_date   # go up one level, then into utils
from .parser import parse_input           # same subpackage
```

Relative import syntax:
- `.` — current package
- `..` — parent package
- `...` — grandparent package

## Package Structure Best Practices

### Keep `__init__.py` minimal

Only expose a curated public API. Avoid imports that trigger heavy computation or side effects.

### Use relative imports inside the package

They make the package portable and explicit about internal dependencies.

```python
# Prefer this (relative)
from .utils import helper

# Over this (absolute, fragile if package is renamed)
from mypackage.utils import helper
```

### Group by responsibility, not by type

Organise modules around what they *do*, not what they *are*:

```
# Good — grouped by feature
mypackage/
├── auth/
├── storage/
└── reporting/

# Less good — grouped by type
mypackage/
├── models/
├── helpers/
└── exceptions/
```

### Document your public API.

Use `__all__` in `__init__.py` to declare exactly what gets exported:

```python
# mypackage/__init__.py
__all__ = ["ClassA", "helper_fn"]
```

>[!NOTE]
>This controls what `from mypackage import *` exposes and signals intent to other developers.

**Add a top-level `__version__`** for packages you distribute:

```python
# mypackage/__init__.py
__version__ = "1.0.0"
```

### One concern per module

If a module is getting long or hard to name, it probably needs to be split or promoted into a subpackage.
