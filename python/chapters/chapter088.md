# Development Workflow

This document covers Python 3 development workflow best practices: setting up new projects, managing dependencies, testing in clean environments, and deploying to production. It is intended for solo developers and teams alike.

## Setting Up New Projects

Start every project with a virtual environment and a locked dependency file. This keeps system Python clean and ensures reproducibility.

```bash
mkdir my_project && cd my_project
python3 -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install --upgrade pip
```

Initialize version control immediately:

```bash
git init
echo ".venv/" >> .gitignore
echo "__pycache__/" >> .gitignore
echo "*.pyc" >> .gitignore
```

## Environment Setup Checklist

- [ ] Python 3.11+ installed (`python3 --version`)
- [ ] Virtual environment created and activated
- [ ] `pip` up to date
- [ ] `requirements.txt` or `pyproject.toml` present
- [ ] `.gitignore` excludes `.venv/` and cache files
- [ ] Environment variables stored in `.env` (never committed)

## Dependency Installation Workflow

### Adding New Dependencies

Install a package, then immediately pin it:

```bash
pip install requests
pip freeze > requirements.txt
```

For projects using `pyproject.toml` (recommended):

```bash
pip install requests
# Add to [project] dependencies in pyproject.toml manually
# or use a tool like `uv` or `poetry` to manage this automatically
```

### Updating Dependencies

Update a single package safely:

```bash
pip install --upgrade requests
pip freeze > requirements.txt
```

Update all packages (use with caution, always test after):

```bash
pip list --outdated
pip install --upgrade $(pip list --outdated --format=columns | tail -n +3 | awk '{print $1}')
pip freeze > requirements.txt
```

## Testing in Clean Environments

Never assume your local env matches production. Test installs from scratch regularly:

```bash
deactivate
rm -rf .venv
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m pytest
```

Use `tox` to test across multiple Python versions:

```bash
pip install tox
# Configure tox.ini, then:
tox
```

## Sharing Projects with Others

Always commit `requirements.txt` (or `pyproject.toml` + lock file). Never commit `.venv/`. Include a `README.md` with setup instructions.

Minimal onboarding block for your `README.md`:

```markdown
## Setup

\`\`\`bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
\`\`\`
```

## Onboarding New Developers

1. Clone the repo.
2. Create and activate a virtual environment.
3. Run `pip install -r requirements.txt`.
4. Copy `.env.example` to `.env` and fill in secrets.
5. Run tests: `python -m pytest`.

Provide a `Makefile` to reduce friction:

```makefile
setup:
    python3 -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt

test:
    python -m pytest

run:
    python src/main.py
```

## CI/CD Environment Setup

Example GitHub Actions workflow:

```yaml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.11"
      - run: pip install -r requirements.txt
      - run: python -m pytest
```

Best practices:
- Pin Python version explicitly in CI.
- Cache pip dependencies to speed up runs.
- Run `pip check` to catch dependency conflicts early.
- Store secrets in CI environment variables, never in code.

## Production Deployment Considerations

- Use a pinned `requirements.txt` or a lock file (`poetry.lock`, `uv.lock`).
- Avoid `pip install --upgrade` in production without testing first.
- Use environment variables (not `.env` files) for secrets in production.
- Run as a non-root user inside containers.
- Keep the Docker image minimal — use `python:3.11-slim` as base.
- Use a process manager (`gunicorn`, `uvicorn`) rather than `python app.py`.

Example minimal `Dockerfile`:

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["gunicorn", "src.main:app", "--bind", "0.0.0.0:8000"]
```
