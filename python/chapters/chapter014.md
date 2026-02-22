# Python Documentation
Documentation is a first-class concern in Python. Well-documented code is easier to maintain, reuse, and share. Python provides built-in support for documentation through docstrings, and the ecosystem offers tools to generate, lint, and publish docs automatically.

## Writing Docstrings

A docstring is a string literal placed as the first statement in a module, class, or function. It becomes the `__doc__` attribute of that object.

```python
def add(a: int, b: int) -> int:
    """Return the sum of a and b."""
    return a + b
```

**Multi-line docstrings** open with a summary line, followed by a blank line and further detail:

```python
def fetch_user(user_id: int) -> dict:
    """
    Retrieve a user record by ID.

    Queries the database for the given user_id and returns a
    dictionary with the user's profile data.

    Args:
        user_id: The unique identifier of the user.

    Returns:
        A dict containing user profile fields.

    Raises:
        ValueError: If user_id is not a positive integer.
        UserNotFoundError: If no user matches the given ID.
    """
```

## Docstring Conventions

Python has no single enforced format, but three styles dominate:

**Google style** — readable, minimal punctuation:

```python
Args:
    name (str): The user's name.

Returns:
    str: A greeting string.
```

**NumPy style** — verbose, suited for scientific libraries:

```python
Parameters
----------
name : str
    The user's name.

Returns
-------
str
    A greeting string.
```

**reStructuredText (reST)** — default for Sphinx, older style:

```python
:param name: The user's name.
:type name: str
:returns: A greeting string.
:rtype: str
```

>[!TIP]
>Pick one style per project and stick to it. Configure linters (e.g., `pydocstyle`) to enforce it.

## Comments vs. Docstrings

| | Comments | Docstrings |
|---|---|---|
| Syntax | `# ...` | `"""..."""` |
| Purpose | Explain *how* or *why* code works | Describe *what* an object does |
| Audience | Developers reading the source | Users of the API / `help()` |
| Tooling | Ignored by doc generators | Parsed by Sphinx, pdoc, etc. |

**Use comments to:**
- Clarify non-obvious logic
- Flag TODOs or known issues
- Annotate regex patterns or magic numbers

**Use docstrings to:**
- Document every public module, class, and function
- Provide usage examples via `doctest`
- Feed auto-generated API documentation

## Documentation Best Practices

- **Document public APIs first.** Private helpers can have minimal or no docstrings.
- **Write docstrings before or during coding**, not after — they clarify intent early.
- **Keep the summary line ≤ 72 chars** and self-contained.
- **Include examples** using `doctest` format where practical:

```python
  >>> add(2, 3)
  5
```

- **Use type hints** alongside docstrings — they reduce redundancy and enable static analysis:

```python
  def greet(name: str) -> str:
      """Return a personalized greeting."""
```

- **Run linters**: `pydocstyle`, `flake8-docstrings`, or `ruff` can enforce style rules automatically.
- **Generate HTML docs** with [Sphinx](https://www.sphinx-doc.org/) or [pdoc](https://pdoc.dev/) from your docstrings.
- **Keep docs in sync with code** — stale documentation is worse than none.
