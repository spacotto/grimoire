# Environment Variables

Environment variables are **key-value pairs** that live outside your code and are used to pass **configuration and secrets to your application at runtime**. This doc covers how to read, set, and manage them in Python 3, along with best practices for security and portability. 

## What are Environment Variables?

Environment variables are named values stored in the process environment, outside of your application's source code. They're set by the shell, the OS, or a deployment platform, and inherited by child processes. Common uses are API keys, database URLs, feature flags, and application mode (DEBUG, PRODUCTION).

```shell
# Shell — setting before running a script
DATABASE_URL=postgres://localhost/mydb python app.py
```

## Reading Environment Variables (os.environ)

Python exposes the current environment via `os.environ`, a dict-like object populated at startup.

```python
import os

# Raises KeyError if not set
db_url = os.environ["DATABASE_URL"]

# Returns None (or a default) if not set — preferred
db_url = os.environ.get("DATABASE_URL")
db_url = os.environ.get("DATABASE_URL", "sqlite:///default.db")

# Check if a variable exists
if "API_KEY" in os.environ:
    ...
```

>[!TIP]
>Prefer `.get()` with a sensible default over direct access. It avoids crashes on missing variables in dev/test environments.

## Setting Environment Variables

You can set variables at runtime within Python. They affect the current process and any subprocesses spawned after.

```python
import os

os.environ["LOG_LEVEL"] = "debug"   # set
del os.environ["LOG_LEVEL"]         # unset
os.putenv("TEMP_DIR", "/tmp")       # low-level — prefer os.environ
```

>[!WARNING]
>Changes to os.environ are not persisted; they disappear when the process ends. To set variables permanently, do it in your shell profile or a .env file.

For local dev, use a `.env` file with `python-dotenv`:

```ini
# .env
DATABASE_URL=postgres://localhost/mydb
DEBUG=true
```

```python
from dotenv import load_dotenv
load_dotenv()   # loads .env into os.environ
```

## Environment Variables in Different OS

| Task | Linux / macOS | Windows (cmd) | Windows (PS) |
| :--- | :--- | :--- | :--- |
| **Set (session)** | `export KEY=value` | `set KEY=value` | `$env:KEY="value"` |
| **Read** | `echo $KEY` | `echo %KEY%`	| `$env:KEY` |
| **Unset** | `unset KEY` | `set KEY=` | `Remove-Item Env:KEY` |
| **List all** | `env` | `set` | `Get-ChildItem Env:` |

## Environment Variable Naming Conventions

By convention, environment variable names comply with the following rules.

1. The keys are written in UPPER_SNAKE_CASE:

```ini
DATABASE_URL=...
AWS_ACCESS_KEY_ID=...
MAX_RETRIES=3
```

2. Prefix by app or service to avoid collisions:

```ini
MYAPP_DEBUG=true
MYAPP_PORT=8080
```

3. Avoid lowercase names: they're technically valid but not conventional and can be confused with shell variables.

4. Never use spaces or hyphens in names.

## When to Use Environment Variables

| 👍 Use for | 👎 Avoid for |
| :--- | :--- |
| API keys, tokens, passwords | Complex structured config (use a config file) |
| Database / service URLs	| Data that changes at runtime |
| Runtime mode (`DEBUG`, `ENV`) | Large values (env vars have size limits) |
| Port numbers, hostnames | Non-string types without conversion |
| Cloud / CI/CD config | Secrets shared across many services (use a vault) |

## Security with Environment Variables

>[!WARNING]
>Never commit secrets! Add `.env` to `.gitignore` immediately!

1. Add the following items to the `.gitignore`:

```ini
.env
.env.local
*.env
```

2. Validate required secrets at startup (fail fast):

```python
import os, sys

REQUIRED = ["DATABASE_URL", "SECRET_KEY", "API_KEY"]
missing = [k for k in REQUIRED if not os.environ.get(k)]
if missing:
    sys.exit(f"Missing env vars: {', '.join(missing)}")
```

3. Don't log secrets, even partially:

```python
print(f"Loaded key: {os.environ['API_KEY']}")       # bad
print("API key loaded.")                            # good
```

4. Type-cast and validate values:

```python
import os
port = int(os.environ.get("PORT", "8080"))
debug = os.environ.get("DEBUG", "false").lower() == "true"
```

>[!TIP]
>For production secrets at scale, use a dedicated secrets manager (AWS Secrets Manager, HashiCorp Vault, GCP Secret Manager) rather than raw environment variables.

## Environment Variable Precedence

When the same variable is defined in multiple places, a typical resolution order (highest → lowest priority):

1. Process-level (set in Python at runtime):

```python
os.environ["PORT"] = "9000"
```

2. Shell export (before launching Python):

```python
export PORT=8080
```

3. `.env` file loaded via `python-dotenv`:

```python
PORT=7000
```

4. System-level / OS defaults

5. `python-dotenv` won't overwrite existing variables by default. Pass `override=True` to force it:

```python
load_dotenv(override=True)   # .env wins over existing env
```

>[!TIP]
>Rule of thumb: process-level > shell > `.env` file > OS defaults. Design your config loading with this order in mind.
