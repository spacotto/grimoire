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
