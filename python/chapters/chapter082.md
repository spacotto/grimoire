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

## Installation

Install all dependencies from lockfile:

```bash
poetry install
```

Install without dev dependencies (e.g. production):

```bash
poetry install --without dev
```

Install only specific groups:

```bash
poetry install --with docs
```

Sync (remove packages not in lockfile):

```bash
poetry install --sync
```

>[!NOTE]
>On first run, Poetry creates a virtualenv automatically if one doesn't exist.

## Add & Remove

Add a runtime dependency:

```bash
poetry add requests
```

Add with version constraint:

```bash
poetry add "httpx>=0.27"
```

Add to a dependency group:

```bash
poetry add pytest --group dev
```

Add an optional dependency:

```bash
poetry add boto3 --optional
```

Remove a dependency:

```bash
poetry remove requests
```

>[!NOTE]
>Both commands update `pyproject.toml` and `poetry.lock` automatically, then sync the virtualenv.

## Update

Update all dependencies within constraints:

```bash
poetry update
```

Update specific packages only:

```bash
poetry update requests httpx
```

Preview what would change without applying:

```bash
poetry update --dry-run
```

>[!NOTE]
>`poetry update` re-resolves dependencies within the version ranges in `pyproject.toml` and rewrites the lockfile. It does not change the constraints themselves.

## Dependency groups (Organisation)

1. `dev`: linters, formatters, type checkers (ruff, mypy)
2. `test`: test runners and fixtures (pytest, factory-boy)
3. `docs`: doc generators (mkdocs, sphinx)

```ini
[tool.poetry.group.test.dependencies]
pytest = "^8.0"
pytest-cov = "^5.0"

[tool.poetry.group.docs.dependencies]
mkdocs = "^1.6"
mkdocs-material = "^9.5"
```

Install everything, including all groups:

```bash
poetry install --with test,docs
```

Make a group optional (not installed by default):

```ini
[tool.poetry.group.docs]
optional = true
```

## Poetry virtual environment management

Show active virtualenv info:

```bash
poetry env info
```

List all envs for this project:

```bash
poetry env list
```

Create env with a specific python version:

```bash
poetry env use python3.12
```

Remove a virtualenv:

```bash
poetry env remove python3.11
```

By default, Poetry creates virtualenvs in a central cache directory. To keep the env inside the project folder:

```bash
poetry config virtualenvs.in-project true
# creates: .venv/ in your project root
```

## `poetry run`

Execute a command inside the project's virtualenv without activating it manually.

Run a script:

```bash
poetry run python src/main.py
```

Run tests:

```bash
poetry run pytest
```

Run a tool installed as a dependency:

```bash
poetry run ruff check .
```

Open a shell inside the venv:

```bash
poetry shell
# exit with: exit or deactivate
```

## Publishing packages with Poetry

build source distribution + wheel:

```bash
poetry build
# outputs: dist/my-package-0.1.0.tar.gz
#          dist/my-package-0.1.0-py3-none-any.whl
```

Configure PyPI token (one-time):

```bash
poetry config pypi-token.pypi YOUR_API_TOKEN
```

Publish to PyPI:

```bash
poetry publish
```

Build and publish in one step:

```bash
poetry publish --build
```

Publish to a private/test registry:

```bash
poetry config repositories.testpypi https://test.pypi.org/legacy/
poetry publish --repository testpypi
```

>[!IMPORTANT]
>Always bump the `version` field in `pyproject.toml` before publishing. Use `poetry version patch|minor|major` to bump it automatically.
