# Python Project Structure

A well-organised Python project makes collaboration easier, tooling smoother, and maintenance less painful. This document covers the conventions most Python projects follow, from directory layout to configuration files.

## Standard Project Layout

Most Python projects share a predictable skeleton:

```
my_project/
├── src/                  # or flat: my_project/ at root
│   └── my_project/
│       ├── __init__.py
│       └── module.py
├── tests/
│   └── test_module.py
├── docs/
├── README.md
├── LICENSE
└── pyproject.toml
```

>[!TIP]
>Stick to this structure unless you have a strong reason not to.

## src/ Layout vs. Flat Layout

### src/ layout

Package lives inside `src/`. Prevents accidentally importing the local package instead of the installed one during testing.

```
src/
└── my_project/
    ├── __init__.py
    └── core.py
```

>[!TIP]
>Use when building a library or distributable package.

### Flat layout

Package sits at the project root. Simpler, fewer directories.

```
my_project/
├── __init__.py
└── core.py
```

>[!TIP]
>Use when building a small app or script-heavy project.

>[!NOTE]
>Modern tools like `setuptools` and `hatch` support both layouts natively.

## Directory Organisation

Keep each directory focused on one concern:

| Directory       | Purpose                                      |
|-----------------|----------------------------------------------|
| `src/`          | Source code (src layout)                     |
| `tests/`        | Test files, fixtures, conftest                |
| `docs/`         | Documentation source (Sphinx, MkDocs, etc.)  |
| `scripts/`      | Utility / maintenance scripts                |
| `.github/`      | CI workflows, issue templates                |

>[!TIP]
>Avoid dumping unrelated files into the package directory.

## tests/ Directory

```
tests/
├── conftest.py           # shared pytest fixtures
├── unit/
│   └── test_core.py
└── integration/
    └── test_api.py
```

**Key points:**

- Mirror the package structure inside `tests/` for easy navigation.
- Use `conftest.py` for fixtures shared across test modules.
- Keep unit tests and integration tests in separate subdirectories.
- Do not ship `tests/` inside the installed package (exclude it in `pyproject.toml`).

## docs/ Directory

```
docs/
├── conf.py               # Sphinx config (if using Sphinx)
├── index.md              # Entry point
├── api/                  # Auto-generated API reference
└── guides/               # How-to guides, tutorials
```

**Common documentation tools:**

- `Sphinx` — feature-rich, standard for large libraries.
- `MkDocs` + `mkdocs-material` — Markdown-native, clean output.
- `pdoc` — zero-config API docs from docstrings.

## Configuration Files Location

Place all configuration at the project root, not buried in subdirectories.

| File                  | Purpose                              |
|-----------------------|--------------------------------------|
| `pyproject.toml`    | Build system, deps, tool config      |
| `setup.cfg`         | Legacy metadata (avoid for new work) |
| `.env`              | Local secrets (never commit)         |
| `.gitignore`        | Git exclusions                       |
| `Makefile`          | Common dev commands (optional)       |

Consolidate tool settings (`pytest`, `mypy`, `ruff`) into `pyproject.toml` to keep the root clean.

## README and Documentation

A `README.md` at the root is mandatory. Cover:

1. **What it does** — one-sentence description.
2. **Install** — `pip install my_project` or editable install instructions.
3. **Quick start** — minimal working example.
4. **Links** — full docs, changelog, contributing guide.

>[!TIP]
>Keep the README short. Detailed content belongs in `docs/`.

## LICENSE Files

Always include a `LICENSE` file. No license = all rights reserved by default, which blocks others from using your code.

**Common choices:**

| License     | Use case                                  |
|-------------|-------------------------------------------|
| MIT         | Permissive, maximum adoption              |
| Apache 2.0  | Permissive + explicit patent grant        |
| GPL 3.0     | Copyleft, derivative works stay open      |
| AGPL 3.0    | Copyleft extended to network services     |

>[!TIP]
>Use `choosealicense.com` if unsure.

## setup.py and pyproject.toml

### pyproject.toml (recommended)

The modern standard. Declare build system, metadata, and tool config in one file.

```toml
[build-system]
requires = ["setuptools>=68"]
build-backend = "setuptools.backends.legacy:build"

[project]
name = "my_project"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = ["requests>=2.31"]
```

### setup.py (legacy)

Only needed for editable installs with older pip or when a build step requires Python logic. Prefer a minimal shim if required:

```python
from setuptools import setup
setup()
```

>[!TIP]
>**Avoid duplicating metadata** between `setup.py` and `pyproject.toml`.

## Project Metadata

Define metadata once, in `pyproject.toml`:

```toml
[project]
name            = "my_project"
version         = "0.1.0"
description     = "A short description"
readme          = "README.md"
license         = { text = "MIT" }
authors         = [{ name = "Your Name", email = "you@example.com" }]
keywords        = ["python", "example"]
classifiers     = [
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: MIT License",
]
requires-python = ">=3.11"

[project.urls]
Homepage   = "https://github.com/you/my_project"
Repository = "https://github.com/you/my_project"
```

>[!TIP]
>PyPI uses these fields for your project page — fill them in properly
from the start.
