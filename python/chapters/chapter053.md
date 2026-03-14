# Absolute Imports

Python needs to know *where* a module lives before it can load it. Absolute imports solve this by specifying the full path from the project's root package — no guessing, no ambiguity. They are the recommended style in Python 3 and the default when no import mode is explicitly set.

## What Are Absolute Imports?

An absolute import locates a module by its complete path, starting from the top-level package of the project.

```python
# Project structure
# my_project/
# ├── utils/
# │   └── helpers.py
# └── main.py

# In main.py — absolute import
import utils.helpers
```

Python resolves this path against `sys.path`, which normally includes the project root. The full chain is always visible in the import statement itself.

## Full Import Paths

The import statement mirrors the directory structure exactly: `package.subpackage.module`.

```python
# Directory: my_project/data/parsers/csv_parser.py

import my_project.data.parsers.csv_parser
```

Every segment maps to a directory or file on disk. There are no shortcuts, no dots indicating "go up one level".

## `from package.module import name`

The `from … import` form lets you pull a specific name (function, class, constant) into the current namespace without importing the whole module object.

```python
# Import only what you need
from utils.helpers import format_date
from data.validators import validate_email, validate_phone

# Use directly — no module prefix required
result = format_date("2026-03-14")
```

This keeps call sites clean while keeping the origin of the name traceable back to its full path.

## Clarity and Explicitness

Absolute imports make the source of every dependency immediately obvious — to the reader, to the linter, and to the IDE.

```python
# ✅ Absolute — origin is unambiguous
from analytics.reports.monthly import generate_summary

# ❌ Relative — requires knowing the current file's location
from ..reports.monthly import generate_summary
```

Because the full path is always written out, moving a file to a different directory does not silently change what gets imported.

## When to Use Absolute Imports

**Use absolute imports:**
- In application code and scripts (the common case).
- When importing across packages within the same project.
- When the module will be run directly (`python my_module.py`).
- When collaborating — explicit paths reduce onboarding friction.

**Relative imports are acceptable:**
- Inside a package, when two sibling modules are tightly coupled and the package is designed to be relocated as a unit.
- In library code where the internal structure must stay portable.

Python 3 disables implicit relative imports entirely; any relative import must use the explicit dot syntax (`from . import sibling`).

## Absolute Import Best Practices

**Keep imports at the top of the file.**

```python
# ✅ Standard position
import os
from pathlib import Path
from my_project.utils.helpers import slugify
```

**Import the module, not a deep attribute, when the path is very long.**

```python
# Easier to read at the call site
from my_project.data import validators
validators.validate_email(email)
```

**Avoid wildcard imports.**

```python
# ❌ Pollutes the namespace and hides origins
from utils.helpers import *

# ✅ Name what you need
from utils.helpers import format_date, parse_url
```

**Set up `sys.path` or `PYTHONPATH` correctly** so absolute paths resolve without hacks. The cleanest way is to install the project as a package (even in development mode):

```bash
pip install -e .
```

**One import per line** for readability and clean version-control diffs.

```python
# ✅
import os
import sys

# ❌
import os, sys
```
