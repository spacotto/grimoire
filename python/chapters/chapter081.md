# Package Management with pip

This document covers pip, Python's standard package manager, from basics through best practices.

## What is `pip`?

`pip` is the **package installer for Python**. It fetches packages from `PyPI` (Python Package Index) and installs them into your environment. It ships with `Python 3.4+` and is the standard tool for dependency management.

```bash
# verify pip is available
pip --version
pip 24.0 from /usr/lib/python3/dist-packages/pip (python 3.12)

# always prefer the module form to avoid path ambiguity
python3 -m pip --version
```

>[!TIP]
>Use `python3 -m pip` instead of bare `pip` to ensure you're using the pip tied to the active Python interpreter.

## Installing packages

Install one or more packages by name. pip resolves and installs dependencies automatically.

Install a package:

```bash
pip install requests
```

Install a specific version:

```bash
pip install requests==2.31.0
```

Install multiple packages at once:
```bash
pip install flask sqlalchemy celery
```

Install from a local directory (editable/dev mode):

```bash
pip install -e .
```

>[!TIP]
>Always install inside a virtual environment (`venv` or `virtualenv`) to keep project dependencies isolated.

## Uninstalling packages

Remove a package and its metadata. Dependencies are not removed automatically.

Uninstall a package (prompts for confirmation):

```bash
pip uninstall requests
```

Skip the confirmation prompt:
```bash
pip uninstall requests -y
```

Uninstall multiple packages:
```bash
pip uninstall flask sqlalchemy -y
```

## Listing installed packages

See everything installed in the current environment.

List all installed packages with versions:

```bash
pip list
```

Show only outdated packages:

```bash
pip list --outdated
```

Output as JSON:

```bash
pip list --format=json
```

## Upgrading packages

Upgrade a package to the latest compatible version:

```bash
pip install --upgrade requests
```

Upgrade pip itself:

```bash
python3 -m pip install --upgrade pip
```

Upgrade multiple packages:

```bash
pip install --upgrade flask jinja2
```

>[!WARNING]
>Upgrading without version constraints can introduce breaking changes. Pin versions in production environments.

## pip freeze and requirements files

`pip freeze` outputs all installed packages with exact versions (the canonical way to snapshot an environment).

Print all installed packages in requirements format:

```bash
pip freeze
certifi==2024.2.2
charset-normalizer==3.3.2
idna==3.6
requests==2.31.0
urllib3==2.2.1
```

Save snapshot to a file:

```bash
pip freeze > requirements.txt
```

## `requirements.txt` format

A plain text file, one dependency per line. Supports comments and a range of version specifiers.

```ini
# requirements.txt

# exact pin
requests==2.31.0

# minimum version
flask>=3.0

# compatible release (allows patch upgrades)
sqlalchemy~=2.0.0

# range
celery>=5.3,<6.0

# no constraint (latest)
click
```

### Operators
- `==`: Exact version
- `>=`: Minimum version
- `<=`: Maximum version
- `~=`: Compatible release
- `!=`: Exclude version

## Installing from requirements files

Install everything listed in requirements.txt:

```bash
pip install -r requirements.txt
```

Separate dev and production requirements:

```bash
pip install -r requirements.txt -r requirements-dev.txt
```

requirements.txt

```bash
flask==3.0.3
sqlalchemy==2.0.29
celery==5.3.6
```

requirements-dev.txt

```bash
pytest==8.1.1
black==24.3.0
ruff==0.3.5
```
