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
