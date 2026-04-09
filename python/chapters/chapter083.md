# Dependency Management

Every non-trivial Python project relies on external packages. Managing those packages (what they are, which versions to use, and how to install them consistently) is dependency management. Get it wrong, and you get broken environments, security holes, or builds that work on your machine but nowhere else. This document covers the core concepts and tools you need to do it right.

## Understanding Dependencies

A **dependency** is any external package your project needs to run. You declare them, so tools like `pip` know what to install.

```python
# Your code uses requests — that's a dependency
import requests
response = requests.get("https://example.com")
```

>[!WARNING]
>Without declaring `requests` as a dependency, anyone cloning your project will hit an `ImportError` immediately.

## Direct vs. Transitive Dependencies

| Direct dependencies | Transitive (or indirect) dependencies |
| :--- | :--- |
| Packages you explicitly import and use. | Packages your dependencies depend on. You don't use them directly, but they must be installed for your direct dependencies to work. |

```
your project
├── requests          ← direct dependency
│   ├── urllib3       ← transitive dependency
│   ├── certifi       ← transitive dependency
│   └── charset-normalizer  ← transitive dependency
```

>[!NOTE]
>You only declare direct dependencies. Your package manager resolves the rest.

## Dependency Resolution

When you run `pip install`, pip works out which version of every package (direct and transitive) satisfies all constraints simultaneously. This is **dependency resolution**.

```bash
pip install flask django
```

If `flask` requires `werkzeug>=2.0` and `django` requires `werkzeug<3.0`, pip finds a `werkzeug` version in the `>=2.0, <3.0` range. If no version satisfies all constraints, resolution fails.

## Version Constraints and Semantic Versioning

Most Python packages use **Semantic Versioning**: `MAJOR.MINOR.PATCH`

| Part | Meaning | Example |
|------|---------|---------|
| MAJOR | Breaking changes | `2.0.0` |
| MINOR | New features, backward-compatible | `1.3.0` |
| PATCH | Bug fixes | `1.2.5` |

You specify constraints in your dependency files:

```
requests==2.31.0      # exact version
requests>=2.28.0      # minimum version
requests>=2.28,<3.0   # version range
requests~=2.28.0      # compatible release: >=2.28.0, <2.29.0
requests~=2.28        # compatible release: >=2.28, <3.0
```

>[!TIP]
>Use `~=` (compatible release) when you want bug fixes but not breaking changes. Use exact pins (`==`) for maximum reproducibility.

## Dependency Conflicts

A **conflict** occurs when two packages require incompatible versions of the same dependency.

```
package-a requires numpy>=1.24
package-b requires numpy<1.20
```

There is no version of `numpy` that is both `>=1.24` and `<1.20`. pip will raise a `ResolutionImpossible` error.

**Options when conflicts arise:**
- Upgrade one of the conflicting packages (if a newer version relaxed its constraint)
- Use a virtual environment per project so conflicts don't bleed across projects
- Consider replacing one of the conflicting packages with an alternative

```bash
# Always work in a virtual environment
python -m venv .venv
source .venv/bin/activate   # Linux/macOS
.venv\Scripts\activate      # Windows
```

## Pinning Dependencies

**Pinning** means locking a dependency to a specific version.

```
# Unpinned — installs whatever is latest
requests

# Pinned — always installs exactly this version
requests==2.31.0
```

**Why pin?**
- Reproducible installs across machines and time
- Prevents surprise breakage from upstream updates
- Easier to audit exactly what's running in production

**Trade-off:** pinned deps don't automatically get security patches. You must update pins deliberately and test.

## `requirements.txt` vs. `pyproject.toml`

### `requirements.txt`

The traditional format. A plain list of packages, one per line.

```txt
# requirements.txt
flask==3.0.3
sqlalchemy>=2.0,<3.0
python-dotenv==1.0.1
```

Install with:

```bash
pip install -r requirements.txt
```

✔️ Good for: 
- simple scripts
- deployment environments
- Docker images

✖️ Not good for:
- Declaring package metadata
- Build systems
- Nuanced dependency ranges meant for library consumers

### `pyproject.toml`

The modern standard (PEP 517/518/621). Combines project metadata, build configuration, and dependencies in one file.

```toml
# pyproject.toml
[project]
name = "my-app"
version = "1.0.0"
requires-python = ">=3.11"
dependencies = [
    "flask>=3.0",
    "sqlalchemy>=2.0,<3.0",
    "python-dotenv>=1.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=8.0",
    "ruff>=0.4",
]
```

Install with:

```bash
pip install .                  # install the project
pip install ".[dev]"           # include dev extras
```

Good for: libraries, applications, anything that should be packaged or published. Supported by `pip`, `poetry`, `hatch`, `pdm`, and others.

>[!TIP]
>Use `pyproject.toml` for new projects. Use `requirements.txt` when you need a flat, tool-agnostic install list (e.g., CI pipelines, Docker).

## Lock Files

A **lock file** records the exact resolved versions of every package — direct and transitive — along with their checksums.

| Tool | Lock file |
|------|-----------|
| pip + pip-tools | `requirements.lock` or `requirements.txt` (compiled) |
| Poetry | `poetry.lock` |
| PDM | `pdm.lock` |
| uv | `uv.lock` |

Example with `pip-tools`:

```bash
# Install pip-tools
pip install pip-tools

# Compile requirements.in → requirements.txt (the lock file)
pip-compile requirements.in

# Install from the lock file
pip-sync requirements.txt
```

The compiled `requirements.txt` looks like:

```txt
# Generated by pip-compile
certifi==2024.2.2
    # via requests
charset-normalizer==3.3.2
    # via requests
requests==2.31.0
    # via -r requirements.in
urllib3==2.2.1
    # via requests
```

**Commit lock files to version control.** They are the source of truth for reproducible installs.

## Reproducible Builds

A build is **reproducible** when installing dependencies at any point in time produces the exact same environment.

Checklist:

```bash
# 1. Use a virtual environment
python -m venv .venv && source .venv/bin/activate

# 2. Pin all dependencies (use a lock file)
pip-compile pyproject.toml --output-file requirements.lock

# 3. Install from the lock file with hash verification
pip install -r requirements.lock --require-hashes

# 4. Pin your Python version too
echo "3.12.3" > .python-version   # used by pyenv
```

>[!WARNING]
>Without reproducible builds, `pip install` today may pull different package versions than it did last week, silently changing behavior.

## Security Considerations

Dependencies are a common attack surface. Third-party code runs with the same privileges as yours.

### Keep dependencies updated

```bash
# Check for outdated packages
pip list --outdated

# Check for known vulnerabilities
pip install pip-audit
pip-audit
```

### Verify package integrity

```bash
# Install with hash checking (requires hashes in requirements file)
pip install -r requirements.txt --require-hashes
```

### Minimal footprint

Only add packages you actually need. Every dependency is additional attack surface and maintenance burden.

### Watch for typosquatting

`requets`, `djano`, `flsak`: malicious packages with names close to popular ones. Double-check package names before installing.

### Use trusted sources

`pip` installs from PyPI by default. Only add custom indexes if you control them or fully trust them.

```bash
# Verify you know what you're installing
pip show requests           # inspect an installed package
pip index versions requests # list available versions on PyPI
```
