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

## Environment Variables in Different OS

## Environment Variable Naming Conventions

## When to Use Environment Variables

## Security with Environment Variables

## Environment Variable Precedence
