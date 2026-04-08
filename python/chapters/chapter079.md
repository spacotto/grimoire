# Introduction to Python Environments

Python environments define the context in which your code runs — which interpreter version is used and which packages are available. Understanding environments is essential for writing reliable, reproducible Python code, whether you're building a small script or a production service.

## What are Python Environments?

A Python environment is a self-contained directory that holds:

- A specific **Python interpreter** (e.g. `python3.11`)
- A set of **installed packages** (e.g. `numpy`, `requests`)
- Scripts and binaries associated with those packages

When you run `python script.py`, Python uses the active environment to resolve imports and locate dependencies.

```bash
# Check which Python is currently active
which python3
python3 --version

# Check installed packages in the active environment
pip list
```

## Global vs. Local Environments

### Global environment

The system-wide Python installation. Packages installed here are available to all users and all projects on the machine.

```bash
# Installs into the global environment (avoid this for projects)
pip install requests
```

### Local (virtual) environment (venv)

An isolated environment scoped to a single project. Created with `venv` or tools like `virtualenv`, `conda`, or `uv`.

```bash
# Create a virtual environment in the current directory
python3 -m venv .venv

# Activate it (macOS/Linux)
source .venv/bin/activate

# Activate it (Windows)
.venv\Scripts\activate

# Deactivate when done
deactivate
```

>[!IMPORTANT]
>Once activated, `python` and `pip` point to the local environment, not the global one.

## Why Isolated Environments Matter

Using isolated environments per project prevents a class of subtle, hard-to-debug problems. The core issues they solve are:

1. **Environment pollution** — global installs affecting unrelated projects
2. **Dependency conflicts** — incompatible package versions across projects
3. **Reproducibility** — ensuring the same code runs the same way everywhere

## Environment Pollution Problems

Installing packages globally accumulates state over time. A package installed for Project A may silently affect Project B's behavior.

Common symptoms:
- Unexpected `ImportError` after a global `pip install`
- A working script breaks when run on a different machine
- Tests pass locally but fail in CI

```bash
# Bad: pollutes the global environment
pip install flask==2.3.0

# Good: install inside a virtual environment
source .venv/bin/activate
pip install flask==2.3.0
```

## Dependency Conflicts

Different projects often require different versions of the same package. Python can only have one version of a package installed per environment.

```
Project A  →  requires  django==3.2
Project B  →  requires  django==4.2
```

Without isolated environments, installing one breaks the other. With virtual environments, each project carries its own dependency tree. Thus, no conflicts.

## Reproducible Environments

A reproducible environment guarantees that code behaves identically across machines, teammates, and deployment targets. The standard approach is a `requirements.txt` file.

```bash
# Capture the current environment's dependencies
pip freeze > requirements.txt

# Recreate the exact environment elsewhere
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

For stricter reproducibility (locked transitive dependencies), consider
`pip-tools`, `poetry`, or `uv`.

## Development vs. Production Environments

Dev and prod environments serve different purposes and should have different
dependencies.

| Concern              | Development       | Production          |
|----------------------|-------------------|---------------------|
| Debuggers/profilers  | Yes               | No                  |
| Test frameworks      | Yes               | No                  |
| Hot-reloading        | Yes               | No                  |
| Core app packages    | Yes               | Yes                 |

A common pattern is to split requirements into two files:

```
requirements.txt          # production dependencies only
requirements-dev.txt      # includes requirements.txt + dev tools
```

```bash
# requirements-dev.txt example
-r requirements.txt
pytest
black
ipython
```

```bash
# Install everything for local development
pip install -r requirements-dev.txt

# Install only what production needs
pip install -r requirements.txt
```

Keeping these separate reduces attack surface, image size, and build time in
production.
