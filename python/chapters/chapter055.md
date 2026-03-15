# Absolute vs. Relative Imports

Python offers two ways to import modules: **absolute** and **relative**. Choosing between them affects readability, portability, and maintainability. This note covers when and why to prefer one over the other.

## The Great Pathway Debate

An **absolute import** specifies the full path from the project root:

```python
# Absolute
from myproject.utils.helpers import format_date
```

A **relative import** specifies the path relative to the current module:

```python
# Relative
from ..utils.helpers import format_date
```

Both are valid — but they serve different contexts.

## Advantages of Absolute Imports

- **Unambiguous**: the full path makes it clear where a module lives.
- **IDE-friendly**: most editors and linters resolve them more reliably.
- **Portable**: safe to use in scripts, notebooks, and top-level modules.
- **Easier to grep**: searching for `myproject.utils` across a codebase is straightforward.

```python
from myproject.data.parsers import CSVParser
from myproject.core.stream import DataStream
```

## Advantages of Relative Imports

- **DRY inside packages**: no need to repeat the package name across sibling modules.
- **Refactor-friendly within a package**: renaming the package doesn't break internal imports.
- **Signals intent**: a `.` or `..` immediately communicates "this is an internal reference."

```python
# Inside myproject/data/adapters.py
from .parsers import CSVParser       # sibling module
from ..core.stream import DataStream # one level up
```

## Clarity vs. Conciseness

| | Absolute | Relative |
|---|---|---|
| Verbosity | Higher | Lower |
| Clarity for new readers | High | Requires knowing the file tree |
| Risk of name shadowing | Low | Low (within a package) |

Absolute imports win on **clarity**; relative imports win on **conciseness** inside tightly coupled packages.

## Refactoring Considerations

- **Renaming a package?** Absolute imports break across the whole codebase — one search-and-replace required.
- **Moving a module within a package?** Relative imports break locally — only the moved file needs updating.
- **Moving a module across packages?** Both styles require updates; absolute imports make the scope of change more obvious.

>[!TIP]
>Rule of thumb: relative imports are stable *within* a package boundary; absolute imports are stable *across* it.

## Project Size Considerations

| Project size | Recommended style |
|---|---|
| Script / single file | Absolute only (no package structure) |
| Small package (1–2 submodules) | Either; prefer absolute for simplicity |
| Medium package (3–10 submodules) | Relative for internals, absolute for cross-package |
| Large / multi-package project | Absolute throughout for traceability |

## Team Preferences

- In solo or small projects, either style works — **consistency matters most**.
- In larger teams, absolute imports reduce onboarding friction: a new contributor can read any file without mentally resolving `..` chains.
- If your team uses a monorepo with multiple packages, absolute imports prevent accidental cross-package coupling through relative paths.

## PEP 8 Recommendations

[PEP 8](https://peps.python.org/pep-0008/#imports) and [PEP 328](https://peps.python.org/pep-0328/) are explicit:

>[!NOTE]
>*"Absolute imports are recommended, as they are usually more readable and tend to be better behaved."*

- **Prefer absolute imports** in all new code.
- **Relative imports are acceptable** for intra-package references when the hierarchy is deep and absolute paths would be excessively verbose.
- **Never use implicit relative imports** (removed in Python 3):

```python
# Python 2 only — do NOT use in Python 3
import helpers  # ambiguous: local file or installed package?

# Python 3 — always explicit
from . import helpers  # explicit relative
from myproject.utils import helpers  # explicit absolute
```

## Quick Reference
```python
# ✅ Absolute — preferred default
from myproject.core.stream import DataStream
from myproject.data.adapters import CSVAdapter

# ✅ Relative — acceptable within a package
from .adapters import CSVAdapter          # same package
from ..core.stream import DataStream      # parent package

# ❌ Avoid — implicit relative (Python 2 legacy, invalid in Python 3)
import stream
```
