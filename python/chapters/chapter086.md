# Security and .gitignore

Source code is often public. Credentials, tokens, and private keys must never be. This guide covers what to exclude from Git, how to structure a `.gitignore`, and how to handle secrets safely in Python 3 projects.

## Never commit secrets

>[!CAUTION]
>Once a secret is pushed to a remote repository (even briefly) it should be considered compromised. Git history is permanent; deleting the file does not remove it from prior commits!

Common mistakes to avoid:
- Hardcoding API keys or passwords in `.py` files
- Committing `.env` files that contain credentials
- Leaving tokens in Jupyter notebooks (`.ipynb`) before pushing

## The .gitignore file

A `.gitignore` file at the project root tells Git which files and directories to ignore. It uses glob-style pattern matching. Always create one before the first commit.

```bash
# create at project root
touch .gitignore
git add .gitignore
git commit -m "add .gitignore"
```

## .gitignore patterns

Patterns are matched relative to the location of the `.gitignore` file.

```gitignore
# ignore a specific file
secrets.json

# ignore all .env files
*.env

# ignore a directory and everything inside it
logs/

# ignore nested files by extension
**/*.key

# negate: track one exception
!.env.example
```

## Protecting .env files

>[!TIP]
>Use a `.env` file to store secrets locally. Load it with `python-dotenv`. Commit only a `.env.example` with dummy values as documentation.

```gitignore
# .gitignore
.env
.env.*
!.env.example
```

```python
# usage in Python
from dotenv import load_dotenv
import os

load_dotenv()
api_key = os.getenv("API_KEY")
```

## Protecting virtual environments

Virtual environments are large, reproducible from `requirements.txt`, and often contain cached credentials. Never commit them.

```gitignore
# common venv directory names
.venv/
venv/
env/
__pycache__/
*.pyc
```

## API keys and credentials

>[!CAUTION]
>**Never hardcode** credentials directly in source files.

```python
# wrong
client = Client(api_key="sk-abc123...")

# correct
client = Client(api_key=os.getenv("API_KEY"))
```

>[!TIP]
>For production, prefer a secrets manager (AWS Secrets Manager, GCP Secret Manager, HashiCorp Vault) over environment variables on shared hosts.

## Security best practices

- Use the GitHub secret-scanning or `git-secrets` pre-commit hook to catch leaks before push
- Restrict secret access by the principle of least privilege: each service gets only the credentials it needs
- Never log secrets; sanitise error messages and stack traces
- Store `.gitignore` in version control; treat it as a project artefact
- Review pull requests for accidental credential exposure

## Environment-specific secrets

>[!CAUTION]
>Use separate credentials per environment. Never reuse production keys in development.

```
.env.development   # local dev — gitignored
.env.staging       # staging — gitignored
.env.production    # prod — gitignored, managed externally
.env.example       # template — committed, no real values
```

>[!TIP]
>Load the correct file based on an `APP_ENV` variable, or use your deployment platform's secret injection (Render, Railway, Heroku config vars, etc.).

## Secret rotation

- Revoke the old key in the provider's dashboard first
- Update the secret in your secrets manager or environment
- Redeploy; confirm the service is healthy before closing the incident
- If a key was committed, assume it is compromised — rotate even if the commit was private

## Auditing and compliance

Maintain a clear record of which secrets exist, where they are stored, and who has access.

- Use `truffleHog` or `gitleaks` to scan repository history for leaked secrets
- Enable audit logging on your secrets manager
- Document secret ownership — who created it, which service uses it, when it expires
- Schedule periodic access reviews; remove credentials for decommissioned services promptly
