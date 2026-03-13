## `from...import` Statements

Python lets you import specific names directly from a module into your current namespace. Instead of writing `module.function()` every time, a `from...import` statement lets you call `function()` directly. This keeps code shorter — but comes with trade-offs worth knowing.

### Importing Specific Functions

**Syntax:** `from module import function`
```python
from math import sqrt

result = sqrt(16)   # 4.0 — no need to write math.sqrt()
```

You can import any name a module exposes: functions, classes, constants, or other objects. Only the names you list are brought into scope; the rest of the module is not loaded into your namespace.

```python
from os.path import join, exists

path = join("folder", "file.txt")
print(exists(path))
```

### Importing Multiple Items

List names separated by commas to import several at once:

```python
from math import sqrt, pi, floor

print(pi)          # 3.141592653589793
print(floor(2.9))  # 2
```

For long import lists, parentheses let you split across lines cleanly:

```python
from module import (
    ClassA,
    ClassB,
    helper_function,
)
```

### `from module import *`

The wildcard `*` imports **every public name** from a module at once:

```python
from math import *

print(sin(0))   # 0.0
print(cos(0))   # 1.0
```

>[!NOTE]
>"Public" means all names not prefixed with an underscore `_`, or — if the module defines `__all__` — exactly the names listed there.

### Why to Avoid `import *`

Despite the convenience, `import *` is generally discouraged:

- **Hidden origins.** When you read `sqrt(x)` later, there is no immediate way to tell where it came from.
- **Name collisions.** If two modules export a name like `open` or `load`, the second import silently overwrites the first.
- **Polluted namespace.** Dozens of unneeded names land in your scope, making introspection and debugging harder.
- **Tooling breaks.** Linters, IDEs, and static analysers cannot reliably resolve names imported via `*`.

>[!TIP]
>Prefer explicit imports. If a module's public API is large, import the module itself and use dot notation.

### Namespace Considerations

Every name you import with `from...import` lands directly in the **local namespace** — the same space as your own variables. This creates two risks:

**1. Shadowing built-ins**
```python
from os import open   # shadows the built-in open()
```

**2. Overwriting your own names**
```python
result = 42
from mymodule import result   # your value is gone
```

A safe pattern when conflicts are possible is to import the module and keep the dot:
```python
import math
import mymodule

math.sqrt(9)
mymodule.sqrt(9)   # unambiguous
```

Or use an alias to resolve the clash explicitly:
```python
from mymodule import sqrt as my_sqrt

my_sqrt(9)
math.sqrt(9)
```

>[!TIP]
>**Rule of thumb:** import only what you need, name it clearly, and prefer `import module` over `import *` whenever the namespace matters.
