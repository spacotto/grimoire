# The `__init__.py`

`__init__.py` is the file that turns a plain directory into a Python package. It runs automatically when the package is imported, making it the ideal place to shape what the outside world sees — and what it doesn't.

## Purpose of `__init__.py`

Without `__init__.py`, Python treats a folder as just a folder. With it, the folder becomes an importable package.

```
mypackage/
    __init__.py      ← makes this a package
    parser.py
    validator.py
```

```python
import mypackage        # triggers mypackage/__init__.py
from mypackage import parser
```

>[!NOTE]
>When Python imports a package, `__init__.py` is the first thing it executes. Everything you put there runs at import time.

## Empty vs. Populated `__init__.py`

**Empty** — the bare minimum. The directory is a package, but nothing is pre-loaded.

```python
# __init__.py
# (empty)
```

Users must import submodules explicitly:

```python
from mypackage.parser import parse_data
```

**Populated** — you control what's available at the package level.

```python
# __init__.py
from .parser import parse_data
from .validator import validate_schema
```

Users can now write:

```python
from mypackage import parse_data
```

>[!NOTE]
>Populated `__init__.py` files flatten the import path and improve usability.

## Controlling Package Interface

`__init__.py` is your package's front door. You decide which rooms are accessible.

```python
# mypackage/__init__.py
from .parser import parse_data
from .validator import validate_schema
# _internal_helper is NOT exposed here — stays private
```

>[!NOTE]
>Internal modules remain accessible via their full path, but a clean `__init__.py` signals: *"use these, not those."*

## Exposing Functions at Package Level

Instead of forcing users to know your internal module structure, surface the key functions directly.

```python
# mypackage/__init__.py
from .parser import parse_data, parse_batch
from .formatter import format_output
```

Before:

```python
from mypackage.parser import parse_data
from mypackage.formatter import format_output
```

After:

```python
from mypackage import parse_data, format_output
```

>[!NOTE]
>The internal structure stays the same — the interface just becomes simpler.

## Package Metadata (`__version__`, `__author__`)

`__init__.py` is the conventional home for package-level metadata.

```python
# mypackage/__init__.py
__version__ = "1.4.2"
__author__  = "Silvia"
__license__ = "MIT"
```

Access it anywhere:

```python
import mypackage

print(mypackage.__version__)   # "1.4.2"
print(mypackage.__author__)    # "Silvia"
```

>[!NOTE]
>This is especially useful for CLI tools, logging, and packaging with `setuptools`.

## Selective Import Exposure

Use `__all__` to declare exactly what gets exported when someone does `from mypackage import *`.

```python
# mypackage/__init__.py
from .parser    import parse_data, parse_batch
from .validator import validate_schema
from .formatter import format_output

__all__ = [
    "parse_data",
    "parse_batch",
    "validate_schema",
    "format_output",
]
```

Anything not in `__all__` is excluded from wildcard imports — even if it's technically importable.

```python
from mypackage import *   # only imports what's in __all__
```

>[!NOTE]
>`__all__` also serves as self-documenting public API: it's an explicit list of what your package officially supports.

## Package-Level vs. Module-Level Access

Without `__init__.py` setup:

```python
from mypackage.parser    import parse_data     # module-level access
from mypackage.validator import validate_schema # module-level access
```

With `__init__.py` setup:

```python
from mypackage import parse_data, validate_schema  # package-level access
```

>[!TIP]
>The rule of thumb: if users need to know *where inside the package* something lives, your `__init__.py` isn't doing its job yet.

## Information Hiding with `__init__.py`

Python has no true private modules, but `__init__.py` lets you express intent clearly.

**Convention 1 — Underscore prefix**

```python
# mypackage/_internals.py  ← underscore signals: not for public use
```

**Convention 2 — Don't expose it in `__init__.py`**

```python
# mypackage/__init__.py
from .parser import parse_data   # exposed ✓
# _internals is NOT imported here  → effectively hidden
```

**Convention 3 — Use `__all__` as the final word**

```python
__all__ = ["parse_data"]  # _internals won't appear in wildcard imports
```

>[!WARNING]
>None of these prevent a determined user from doing `from mypackage._internals import something`, but they communicate clearly: *this is not part of the public API, use at your own risk.*

>[!NOTE]
>**TL;DR** — `__init__.py` is where a package's identity is defined. Keep internal logic in submodules. Use `__init__.py` to expose a clean, intentional interface to the outside world.
