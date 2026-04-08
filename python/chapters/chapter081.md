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

```bash
copied
# install a package
pip install requests

# install a specific version
pip install requests==2.31.0

# install multiple packages at once
pip install flask sqlalchemy celery

# install from a local directory (editable/dev mode)
pip install -e .
```

>[TIP]
>Always install inside a virtual environment (`venv` or `virtualenv`) to keep project dependencies isolated.
