# Python Virtual Environments (`venv`)

Python virtual environments solve a fundamental problem: different projects often need different — sometimes conflicting — package versions. A virtual environment is an isolated Python runtime with its own packages, separate from the system installation. This document covers the built-in `venv` module: how to create, activate, and manage virtual environments, understand their structure, detect them programmatically, and avoid common pitfalls.

## What is a Virtual Environment?

A virtual environment is a self-contained directory that holds:

- A copy (or symlink) of the Python interpreter
- A private `site-packages/` directory for installed packages
- Activation scripts that point your shell to this isolated environment

Without virtual environments, all packages install globally. This creates version conflicts across projects and pollutes the system Python.

```
Project A needs requests==2.28
Project B needs requests==2.20
→ Both can't coexist globally — virtual environments solve this.
```

## The `venv` Module

`venv` has been part of Python's standard library since **Python 3.3**. No installation needed.

```python
# Check it's available
python3 -m venv --help
```

For Python 2 or older workflows, `virtualenv` (a third-party tool) was the standard. Prefer `venv` for all Python 3 projects.

## Creating Virtual Environments

```bash
# Basic syntax
python3 -m venv <env_name>

# Common convention: name the environment .venv
python3 -m venv .venv

# Use a specific Python version (if installed)
python3.11 -m venv .venv

# Exclude pip from the environment (lighter)
python3 -m venv .venv --without-pip

# Give access to system site-packages (rarely needed)
python3 -m venv .venv --system-site-packages
```

>[!TIP]
>**Convention:** Name your environment `.venv` so editors (VS Code, PyCharm) detect it automatically.

## Activating Virtual Environments

Activation modifies your shell's `PATH` to prioritize the environment's Python and scripts.

```bash
# macOS / Linux (bash/zsh)
source .venv/bin/activate

# Windows (Command Prompt)
.venv\Scripts\activate.bat

# Windows (PowerShell)
.venv\Scripts\Activate.ps1

# fish shell
source .venv/bin/activate.fish
```

Once active, your prompt changes:

```
(.venv) user@machine:~/project$
```

All `python` and `pip` commands now reference the virtual environment.

```bash
# Verify
which python        # → /your/project/.venv/bin/python
python --version    # → environment's Python version
```

## Deactivating Virtual Environments

```bash
deactivate
```

This restores your original shell `PATH`. The environment itself is untouched, just no longer active.

## Virtual Environment Structure

```
.venv/
├── bin/                    # Scripts (macOS/Linux)
│   ├── python              # Symlink to Python interpreter
│   ├── pip
│   ├── activate            # Shell activation script
│   └── activate.fish
├── Scripts/                # Scripts (Windows equivalent of bin/)
├── include/                # C headers for compiling extensions
├── lib/
│   └── python3.x/
│       └── site-packages/  # Installed packages live here
└── pyvenv.cfg              # Environment metadata
```

**`pyvenv.cfg`** is the environment's config file:

```ini
home = /usr/bin
include-system-site-packages = false
version = 3.11.4
```

## Detecting Virtual Environments Programmatically

```python
import sys
import os

# Method 1: Check sys.prefix vs sys.base_prefix
# They differ when inside a virtual environment
def in_virtualenv() -> bool:
    return sys.prefix != sys.base_prefix

# Method 2: Check the VIRTUAL_ENV environment variable
# Set automatically on activation
def in_virtualenv_via_env() -> bool:
    return "VIRTUAL_ENV" in os.environ

# Method 3: Combined (more robust)
def is_venv() -> bool:
    return (
        hasattr(sys, "real_prefix")                  # older virtualenv
        or sys.base_prefix != sys.prefix             # venv (Python 3.3+)
    )

print(is_venv())  # True if running inside a virtual environment
```

For practical use, guard a script that must run in a venv:

```python
import sys

if sys.base_prefix == sys.prefix:
    raise EnvironmentError(
        "This script must be run inside a virtual environment.\n"
        "Run: python3 -m venv .venv && source .venv/bin/activate"
    )
```

## Virtual Environment Best Practices

**Do:**
- Name environments `.venv` for universal editor support
- Add `.venv/` to `.gitignore` — never commit the environment
- Pin dependencies with `pip freeze > requirements.txt`
- Create one environment per project
- Store `requirements.txt` at the project root

**Don't:**
- Move or rename the environment directory (paths break)
- Commit environment files to version control
- Rely on the environment being in a specific absolute path

**Typical project workflow:**

```bash
# 1. Create
python3 -m venv .venv

# 2. Activate
source .venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Work on the project...

# 5. Save new dependencies
pip freeze > requirements.txt

# 6. Deactivate when done
deactivate
```

## When to Use Virtual Environments

| Situation | Use venv? |
|---|---|
| Any Python project with external packages | ✅ Always |
| Collaborating with others | ✅ Always |
| Experimenting / learning new libraries | ✅ Yes |
| Running a one-off system script | ⚠️ Optional |
| Deploying to production (Docker, etc.) | ✅ Yes (or containers) |
| System-level Python tools (e.g., `pip`, `ansible`) | ❌ Use pipx instead |

For global CLI tools written in Python, use [`pipx`](https://pipx.pypa.io/): it manages isolated environments per tool automatically.

## Common Virtual Environment Issues

### `pip` installs packages globally instead of locally
**Cause:** Environment isn't active.  
**Fix:** Run `source .venv/bin/activate` first. Check with `which pip`.

### PowerShell: "execution of scripts is disabled"
**Cause:** Windows execution policy blocks `.ps1` scripts.  
**Fix:**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### `python` command not found after activation
**Cause:** Python not in `PATH`, or environment created with a missing interpreter.  
**Fix:** Use `python3` explicitly, or recreate the environment with a valid interpreter path.

### Moving the project folder breaks the environment
**Cause:** `venv` stores absolute paths internally.  
**Fix:** Delete `.venv/` and recreate it. The environment is reproducible from `requirements.txt`.

```bash
rm -rf .venv
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### `ModuleNotFoundError` for an installed package
**Cause:** Package was installed in a different environment or globally.  
**Fix:** Confirm the environment is active (`which python`) and reinstall:
```bash
pip install <package>
```

### Environment uses wrong Python version
**Cause:** `python3` resolved to an unexpected version.  
**Fix:** Specify the version explicitly:
```bash
python3.11 -m venv .venv
```
