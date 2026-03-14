# Relative Imports

Relative imports let modules within the same package reference each other without hardcoding the full package path. They make internal package structure explicit, portable, and resilient to renaming.

## What Are Relative Imports?

A **relative import** locates a module relative to the current file's position in the package hierarchy — as opposed to an **absolute import**, which always starts from a top-level package.

```python
# Absolute import
from mypackage.utils import helper

# Relative import (equivalent, from inside mypackage)
from .utils import helper
```

Relative imports only work inside a **package** (a directory with `__init__.py`). They cannot be used in standalone scripts.

## Dot Notation (`.` and `..`)

| Syntax | Meaning                        |
|--------|--------------------------------|
| `.`    | Current package                |
| `..`   | Parent package                 |
| `...`  | Grandparent package (and so on)|

Each additional dot moves one level up in the package hierarchy.

## `from . import module`

Imports a sibling module — one that lives in the **same package** as the current file.

```
mypackage/
    __init__.py
    parser.py
    validator.py     ← current file
```

```python
# Inside validator.py
from . import parser          # imports the parser module object
from .parser import parse_csv # imports a specific name from parser
```

## `from .. import module`

Imports from the **parent package** — one level up in the hierarchy.

```
mypackage/
    __init__.py
    utils/
        __init__.py
        formatter.py   ← current file
    config.py
```

```python
# Inside formatter.py
from .. import config          # imports config module from mypackage
from ..config import BASE_PATH # imports a specific name
```

## Sibling Module Imports

Sibling modules share the same parent package directory.

```
mypackage/
    __init__.py
    reader.py
    writer.py    ← wants to use reader
```

```python
# Inside writer.py
from .reader import read_lines
```

No need to know the full `mypackage.reader` path — the dot handles it.

## Parent Package Imports

Use `..` to reach utilities or shared resources defined at a higher level.

```
mypackage/
    __init__.py
    helpers.py
    subpackage/
        __init__.py
        processor.py   ← current file
```

```python
# Inside processor.py
from ..helpers import sanitize
```

This keeps `processor.py` decoupled from the package's top-level name.

## When to Use Relative Imports

- **Inside a package**, when modules are tightly related and belong together.
- When the package may be **renamed or moved** — relative paths stay valid.
- To make **intra-package dependencies explicit** at a glance.
- In libraries and reusable packages distributed via `pip`.

Prefer **absolute imports** for cross-package references and in any file intended to run as a top-level script.

## Relative Import Limitations

### Scripts cannot use relative imports

Running a file directly (`python myfile.py`) sets `__name__` to `"__main__"`, which has no package context. Relative imports will raise:

```
ImportError: attempted relative import with no known parent package
```

Fix: run the package with `-m` instead.

```bash
python -m mypackage.subpackage.processor
```

### Requires a proper package structure

Every directory in the chain must have an `__init__.py` (or be a namespace package). A missing `__init__.py` breaks the hierarchy.

### Cannot go above the top-level package

You cannot use `...` to escape the root package. Doing so raises:
```
ImportError: attempted relative import beyond top-level package
```

### Harder to trace in large codebases

Dots give no hint of the actual package name. In deep hierarchies, absolute imports are often clearer to readers unfamiliar with the layout.
