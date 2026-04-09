# Advanced Package Management with Poetry

Poetry is the modern standard for Python dependency management and packaging. It replaces the fragmented `pip` + `setuptools` + `virtualenv` stack with a single, deterministic tool — from declaring dependencies to publishing to PyPI.

## What is Poetry?

Poetry handles the full lifecycle of a Python project: dependency resolution, virtual environment creation, building, and publishing. Everything is declared in one file: `pyproject.toml`.

```
# what poetry manages in one place
dependencies → lockfile → virtualenv → package build → PyPI publis
```

## Poetry vs. pip

| pip | Poetry |
| :--- | :--- |
| pip install requests| | 
| pip freeze > requirements.txt|  |
| Manual venv setup | |
| No build/publish tooling|  |
