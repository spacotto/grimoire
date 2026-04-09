# Advanced Package Management with Poetry

Poetry is the modern standard for Python dependency management and packaging. It replaces the fragmented `pip` + `setuptools` + `virtualenv` stack with a single, deterministic tool — from declaring dependencies to publishing to PyPI.

## What is Poetry?

Poetry handles the full lifecycle of a Python project: dependency resolution, virtual environment creation, building, and publishing. Everything is declared in one file: `pyproject.toml`.

```
# what poetry manages in one place
dependencies → lockfile → virtualenv → package build → PyPI publis
```

## Poetry vs. pip

| | pip | Poetry |
| :--- | :--- | :--- |
| **Add requests** | `pip install requests` | `poetry add requests` | 
| **Dependencies update** | `pip freeze > requirements.txt` | auto-updates pyproject.toml |
| **venv Management** | Manual venv setup | auto-manages venv |
| **Build & Publish tooling** | No build/publish tooling | build + publish built-in |
| **Dependencies resolution** | pip resolves dependencies greedily and doesn't lock sub-dependencies | Poetry uses a SAT solver to guarantee reproducible installs across machines |

## Installing Poetry (Setup)

Official installer (recommended):

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

Verify:

```bash
poetry --version
```

Enable tab completion (bash):

```bash
poetry completions bash >> ~/.bash_completion
```

>[!WARNING]
>Do not install Poetry with pip into your project's virtualenv; it should live in its own isolated environment.

## `pyproject.toml` Format (Config)

The single source of truth for your project. Replaces `setup.py`, `setup.cfg`, and `requirements.txt`.

```ini
[tool.poetry]
name = "my-package"
version = "0.1.0"
description = "A short description"
authors = ["Alice "]

[tool.poetry.dependencies]
python = "^3.11"
requests = "^2.31"
httpx = { version = "^0.27", optional = true }

[tool.poetry.group.dev.dependencies]
pytest = "^8.0"
ruff = "^0.4"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

>[!NOTE]
>Version constraints: `^1.2` allows `>=1.2, <2.0`. `~1.2` allows `>=1.2`, `<1.3. *` allows any version.

## `poetry.lock` Files (Reproducibility)

The lockfile pins every dependency (including transitive ones) to an exact version and hash. Never edit it manually.

>[!TIP]
>Commit `poetry.lock` for reproducible installs for all contributors.

>[!CAUTION]
>Do NOT commit `poetry.lock` for libraries (let users resolve).
>Do commit `poetry.lock` for applications and services.

Regenerate lockfile without installing:

```bash
poetry lock
```

Check if lockfile is consistent with `pyproject.toml`:

```bash
poetry check
```
