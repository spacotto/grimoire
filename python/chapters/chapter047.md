# Introduction to Modules

As programs grow, keeping all code in a single file becomes impractical. Python's module system solves this by letting you split code into separate, reusable files. This note covers what modules are, why they matter, and how Python locates and loads them at runtime.

## What are Modules?

A **module** is any Python file (`.py`) that contains definitions and statements — functions, classes, variables, or runnable code — which can be imported and reused in other scripts.
```python
# greet.py  ← this file *is* a module
def say_hello(name):
    return f"Hello, {name}!"

# main.py
import greet

print(greet.say_hello("Silvia"))  # Hello, Silvia!
```

>[!NOTE]
>Once imported, every name defined in `greet.py` is available through the `greet` namespace.

## Why Use Modules?

| Benefit | What it means in practice |
|---|---|
| **Reusability** | Write a function once; import it anywhere. |
| **Maintainability** | Smaller, focused files are easier to read and fix. |
| **Namespace isolation** | Each module has its own scope — no accidental name collisions. |
| **Selective imports** | Load only what you need with `from module import name`. |
| **Collaboration** | Different team members can own different modules cleanly. |

## Module Files (`.py`)

>[!IMPORTANT]
>Any `.py` file is a valid module. There are no special keywords needed — saving the file is enough.

```python
project/
├── main.py          # entry point
├── utils.py         # utility functions  ← module
└── data_handler.py  # data logic         ← module
```

### Import styles

```python
import utils                    # full module — access as utils.func()
from utils import parse_csv     # single name — access as parse_csv()
from utils import parse_csv as pc   # aliased import
```

>[!CAUTION]
>**Avoid** `from module import *` in production code — it pollutes the local namespace and makes it hard to trace where names come from.

## The Module Search Path

When you write `import utils`, Python does **not** look everywhere on your machine. It searches a specific, ordered list of locations stored in `sys.path`.

```python
import sys
print(sys.path)
```

Typical output (abbreviated):

```python
['',                              # current working directory
'/usr/lib/python312.zip',
'/usr/lib/python3.12',
'/usr/lib/python3.12/lib-dynload',
'/usr/local/lib/python3.12/dist-packages']
```

## How Python Finds Modules

Python searches `sys.path` **in order** and stops at the first match.

```python
import greet
│
▼
sys.modules cache        → already imported? return it immediately.
│  (miss)
▼
Built-in modules         → e.g. sys, builtins
│  (not found)
▼
sys.path entries (left → right)
├── '' (current directory)
├── PYTHONPATH directories
├── Standard library paths
└── site-packages (third-party)
│
▼
Found → compile to bytecode (.pyc) → execute → cache in sys.modules
Not found → ModuleNotFoundError
```

>[!TIP]
>The current directory is checked first, so a local file named `random.py` would shadow the standard-library `random` module.

## Code Organisation Benefits

Modules are the first step toward well-structured Python projects. A common pattern is to group related modules into **packages** (directories with an `__init__.py` file).

```python
my_app/
├── init.py
├── main.py
├── auth/
│   ├── init.py
│   ├── login.py
│   └── tokens.py
└── data/
├── init.py
├── models.py
└── validators.py
```

```python
# importing from a package
from auth.login import authenticate
from data.models import User
```

### Summary of organisational gains

- **Single Responsibility** — each module does one thing well.
- **Testability** — isolated modules are straightforward to unit-test.
- **Discoverability** — a clear folder structure documents itself.
- **Scalability** — adding features means adding modules, not growing one monolithic file.
