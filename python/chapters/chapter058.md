# Module and Package Best Practices

Well-structured Python projects are easier to read, test, and maintain. This guide covers the core conventions for organising code into modules and packages, naming them clearly, managing imports, and writing useful documentation.

## Organising Code into Modules

A **module** is any `.py` file. The goal is to group related functionality together so that each module has a clear, single responsibility.

**Do:**
- Keep one logical theme per module (e.g., `parser.py`, `validator.py`, `models.py`)
- Move code out of scripts into modules once it starts being reused
- Keep modules focused — if a file is growing past ~300–500 lines, consider splitting it

**Avoid:**
- Dumping unrelated utilities into a generic `utils.py` (split by domain instead)
- Circular imports — if A imports B and B imports A, restructure your dependencies

```python
# Good: focused module
# auth/token.py
def generate_token(user_id: int) -> str: ...
def validate_token(token: str) -> bool: ...
```

## When to Create a Package

A **package** is a directory with an `__init__.py` file. Create one when:

- You have multiple related modules that belong together
- Your project grows beyond 2–3 files
- You want a clear public API (exposed through `__init__.py`)

A single-file script doesn't need a package. A library or application with distinct components does.
```
project/
├── main.py
└── auth/
    ├── __init__.py     # makes it a package
    ├── token.py
    └── permissions.py
```

## Flat vs. Nested Package Structures

**Flat structure** — all modules in one package level. Best for small-to-medium projects.
```
myapp/
├── __init__.py
├── models.py
├── routes.py
└── utils.py
```

**Nested structure** — subpackages group related concerns. Best for larger projects with clear domain boundaries.
```
myapp/
├── __init__.py
├── auth/
│   ├── __init__.py
│   └── token.py
└── data/
    ├── __init__.py
    └── parser.py
```

>[!TIP]
>**Rule of thumb:** start flat. Add nesting only when you have a genuine reason (distinct domains, separate teams, distinct deployment units). Deep nesting without cause creates unnecessary import paths.

## Module Naming Conventions

| Rule | Example |
|------|---------|
| Lowercase only | `parser.py` ✓ |
| Words separated by underscores | `data_loader.py` ✓ |
| Short and descriptive | `validator.py` ✓ |
| No hyphens (invalid in imports) | `data-loader.py` ✗ |
| No numbers at the start | `2fast.py` ✗ |

Avoid shadowing standard library names:
```python
# Bad: shadows the built-in
# csv.py  ← will break: import csv

# Good: be specific
# csv_parser.py
```

## Package Naming Conventions

Same rules as modules, with a few additions:

- **Short names are preferred** — `requests`, `flask`, `auth`
- **No underscores in distribution names** (use hyphens in `pyproject.toml`, underscores in the importable name)
- **Avoid generic names** that clash with popular packages

```
# Distribution name (in pyproject.toml):  my-auth-lib
# Importable name (directory):            my_auth_lib/
```

>[!TIP]
>Check [PyPI](https://pypi.org) before naming a package you plan to publish.

## Import Statement Organisation

Follow [PEP 8](https://peps.python.org/pep-0008/#imports): group imports in this order, separated by blank lines:

1. Standard library
2. Third-party packages
3. Local/project imports

Within each group, alphabetical order is conventional (and enforced by tools like `isort`).
```python
# 1. Standard library
import os
import sys
from pathlib import Path

# 2. Third-party
import requests
from pydantic import BaseModel

# 3. Local
from auth.token import generate_token
from data.parser import parse_csv
```

**Prefer explicit imports** over wildcard imports:
```python
# Avoid — pollutes namespace, hides dependencies
from mymodule import *

# Prefer — explicit and traceable
from mymodule import MyClass, helper_function
```

## Avoiding Common Import Pitfalls

**Circular imports**
Modules that import each other will cause `ImportError`. Fix by extracting shared code into a third module, or by moving the import inside a function.
```python
# Workaround: local import to break the cycle
def get_user(user_id: int):
    from auth.permissions import check_access  # imported at call time
    ...
```

**Implicit relative imports**
Always use explicit relative or absolute imports in Python 3:
```python
# Bad (Python 2 style, not valid in Python 3)
import utils

# Good — absolute
from myapp import utils

# Good — explicit relative (within a package)
from . import utils
from .utils import helper
```

**Importing side-effect-heavy modules at module level**
If an import triggers slow or error-prone operations (DB connection, file reads), import lazily:
```python
def load_data():
    import heavy_module  # only imported when function is called
    return heavy_module.fetch()
```

## Documentation for Modules and Packages

**Module docstring** — place at the very top of the file, before any imports:
```python
"""
auth.token
----------
Utilities for generating and validating JWT tokens.

Typical usage:
    from auth.token import generate_token
    token = generate_token(user_id=42)
"""

import os
...
```

**Package docstring** — place in `__init__.py`:
```python
"""
auth
----
Authentication and authorisation utilities.

Submodules:
    token       — JWT generation and validation
    permissions — Role-based access control
"""
```

**What to include in a module docstring:**
- Purpose of the module (one sentence is enough)
- Non-obvious design decisions or constraints
- A short usage example if the API is not self-evident

**What not to include:**
- Information already clear from the code
- Redundant restatements of function signatures (document those in the functions themselves)

Use `__all__` in `__init__.py` to declare the public API explicitly:
```python
# auth/__init__.py
from .token import generate_token, validate_token
from .permissions import check_access

__all__ = ["generate_token", "validate_token", "check_access"]
```

This makes `from auth import *` safe and documents intent clearly.
