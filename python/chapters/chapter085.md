# Configuration Management

Configuration management is how your application handles settings that change between environments, machines, or deployments: things like API keys, database URLs, and feature flags. The goal: keep config out of your code, make it easy to change, and never ship secrets to version control.

## Configuration vs. Code

- **Code** is logic, it doesn't change between environments.  -
- **Configuration** is anything that deals with hostnames, ports, credentials, timeouts, feature toggles.

>[!TIP]
>If you'd need to change a value to run the app somewhere else, it's config — not code.

```python
# ❌ Config baked into code
DB_URL = "postgresql://admin:secret@localhost/mydb"

# ✅ Config loaded from the environment
import os
DB_URL = os.environ["DATABASE_URL"]
```

## The `.env` File Format

A `.env` file is a plain-text file that stores environment variables as `KEY=VALUE` pairs.

```dotenv
# .env
APP_ENV=development
DEBUG=true
DATABASE_URL=postgresql://user:pass@localhost/mydb
SECRET_KEY=supersecretkey123
PORT=8080
```

**Rules:**
- One variable per line
- No spaces around `=`
- Comments start with `#`
- String values don't need quotes (but can have them)
- Never commit `.env` to version control — add it to `.gitignore`

## `python-dotenv` Library

[`python-dotenv`](https://pypi.org/project/python-dotenv/) reads `.env` files and loads them into `os.environ`.

**Install:**

```bash
pip install python-dotenv
```

**Basic usage:**

```python
from dotenv import load_dotenv
import os

load_dotenv()

db_url = os.getenv("DATABASE_URL")
debug = os.getenv("DEBUG", "false").lower() == "true"
```

## Loading `.env` Files

`load_dotenv()` is flexible — it won't override existing environment variables by default.

```python
from dotenv import load_dotenv, dotenv_values
import os

# Load from default .env in current directory
load_dotenv()

# Load from a specific path
load_dotenv("/path/to/.env.production")

# Override existing env vars
load_dotenv(override=True)

# Load into a dict without touching os.environ
config = dotenv_values(".env")
print(config["DATABASE_URL"])
```

**Load order tip:** Call `load_dotenv()` as early as possible — ideally at the top of your entry point (`main.py`, `app.py`, etc.).

## `.env.example` Templates

A `.env.example` (or `.env.template`) is a committed, safe version of your `.env` with placeholder values. It documents required variables without exposing secrets.

```dotenv
# .env.example
APP_ENV=development
DEBUG=true
DATABASE_URL=postgresql://user:password@localhost/dbname
SECRET_KEY=your-secret-key-here
PORT=8080
REDIS_URL=redis://localhost:6379
```

**Workflow:**

```bash
# New developer onboarding
cp .env.example .env
# Then fill in real values in .env
```

>[!TIP]
>Keep `.env.example` up to date whenever you add a new variable.

## Multiple Environment Configurations

For apps with distinct environments (dev, staging, prod), use separate `.env` files.

```
project/
├── .env              # local overrides (gitignored)
├── .env.development  # dev defaults
├── .env.staging      # staging config
├── .env.production   # prod config (gitignored or managed via secrets)
└── .env.example      # template (committed)
```

```python
import os
from dotenv import load_dotenv

env = os.getenv("APP_ENV", "development")
load_dotenv(f".env.{env}")
```

Run with:

```bash
APP_ENV=production python app.py
```

## Development vs. Production Config

| Setting        | Development              | Production                  |
|----------------|--------------------------|-----------------------------|
| `DEBUG`        | `true`                   | `false`                     |
| `DATABASE_URL` | Local DB                 | Managed cloud DB            |
| `SECRET_KEY`   | Any string               | Long, random, rotated       |
| `LOG_LEVEL`    | `DEBUG`                  | `WARNING` or `ERROR`        |
| Config source  | `.env` file              | Environment variables / secrets manager |

```python
import os

DEBUG = os.getenv("DEBUG", "false").lower() == "true"

if DEBUG:
    print("Running in development mode")
```

>[!TIP]
>**In production:** prefer injecting env vars directly via your platform (Railway, Heroku, Docker, Kubernetes) rather than shipping `.env` files.

## Configuration Hierarchies

Config values are often resolved from multiple sources, with a clear precedence order:

```
OS environment variables        ← highest priority
↓
.env file (local overrides)
↓
.env.{environment} file
↓
Default values in code          ← lowest priority
```

```python
from dotenv import load_dotenv
import os

load_dotenv(".env.development")  # base defaults
load_dotenv(".env", override=True)  # local overrides win

PORT = int(os.getenv("PORT", 8080))  # fallback default in code
```

>[!NOTE]
>This lets developers override specific values locally without touching shared config files.

## Configuration Validation

Don't let missing or malformed config cause cryptic runtime errors. Validate early.

**Simple manual validation:**

```python
import os
from dotenv import load_dotenv

load_dotenv()

REQUIRED = ["DATABASE_URL", "SECRET_KEY", "API_KEY"]

missing = [key for key in REQUIRED if not os.getenv(key)]
if missing:
    raise EnvironmentError(f"Missing required config: {', '.join(missing)}")
```

**With Pydantic (recommended for larger apps):**

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    secret_key: str
    debug: bool = False
    port: int = 8080

    class Config:
        env_file = ".env"

settings = Settings()  # Raises ValidationError if required fields are missing
print(settings.database_url)
```

Install: 

```python
pip install pydantic-settings
```

>[!TIP]
>Pydantic validates types, applies defaults, and gives clear error messages — ideal for production apps.

## Secrets Management

For sensitive values (API keys, passwords, tokens), a `.env` file is fine locally, but not enough in production.

**Production options:**

| Tool                        | Best for                          |
|-----------------------------|-----------------------------------|
| AWS Secrets Manager         | AWS-hosted apps                   |
| GCP Secret Manager          | GCP-hosted apps                   |
| HashiCorp Vault             | Multi-cloud, self-hosted           |
| Azure Key Vault             | Azure-hosted apps                  |
| Doppler / Infisical         | Team secret sync, any platform     |

**Fetch a secret at runtime (AWS example):**

```python
import boto3
import json

def get_secret(name: str) -> dict:
    client = boto3.client("secretsmanager", region_name="us-east-1")
    response = client.get_secret_value(SecretId=name)
    return json.loads(response["SecretString"])

secrets = get_secret("myapp/production")
db_url = secrets["DATABASE_URL"]
```

**Key practices:**
- Rotate secrets regularly
- Use least-privilege access (only grant what's needed)
- Never log secret values
- Audit access to secrets
- Prefer short-lived credentials over long-lived static keys
