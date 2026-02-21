# Python 3 — Custom Exceptions
Python's built-in exceptions cover common error scenarios, but real-world applications often need errors that carry domain-specific meaning. Custom exceptions make error handling more precise, readable, and maintainable by giving each failure mode a clear, intentional name.

## When to Create Custom Exceptions

- The built-in exceptions are too generic for your use case.
- You want callers to catch a specific category of error without catching everything else.
- You need to attach extra context (e.g. error codes, field names) to an exception.
- You are building a library or package and want a clean public error API.

## Defining Custom Exception Classes

Subclass `Exception` (or a more specific built-in) and keep the class body minimal unless you need extra behaviour.

```python
class AppError(Exception):
    pass
```

That's enough for a usable custom exception. The name itself communicates intent.

---

## Inheriting from Exception
Always inherit from `Exception`, not `BaseException`. `BaseException` is reserved for system-level events (`KeyboardInterrupt`, `SystemExit`, etc.) that should not be caught by most application code.

```python
# Correct
class ConfigError(Exception):
    pass

# Avoid
class ConfigError(BaseException):   # too broad
    pass
```

## Creating Exception Hierarchies

Group related errors under a common base class. Callers can then catch broadly or narrowly as needed.

```python
class DatabaseError(Exception):
    """Base for all database errors."""

class ConnectionError(DatabaseError):
    """Failed to connect to the database."""

class QueryError(DatabaseError):
    """Invalid or failed query."""
```

```python
try:
    run_query()
except ConnectionError:
    reconnect()
except DatabaseError:
    log_and_abort()
```

## Custom Error Messages

Pass a message to `super().__init__()` so the exception displays clearly.

```python
class ValidationError(Exception):
    def __init__(self, field: str, reason: str):
        super().__init__(f"Validation failed on '{field}': {reason}")
        self.field = field
        self.reason = reason
```

```python
raise ValidationError("email", "invalid format")
# ValidationError: Validation failed on 'email': invalid format
```

## Adding Custom Attributes to Exceptions

Store structured data on the exception so handlers can act programmatically, not just display a string.

```python
class APIError(Exception):
    def __init__(self, status_code: int, message: str):
        super().__init__(message)
        self.status_code = status_code

try:
    call_api()
except APIError as e:
    if e.status_code == 429:
        retry_after_backoff()
```

## Organising Domain-Specific Exceptions

Keep exceptions close to the domain they belong to. Common patterns:

**Single module** — define exceptions at the top of the module they relate to.

**Dedicated file** — for larger projects, use `exceptions.py` or `errors.py` per package.

```
myapp/
    auth/
        exceptions.py   # AuthError, TokenExpiredError, ...
    billing/
        exceptions.py   # PaymentError, InvoiceError, ...
```

Import and raise them from wherever they are needed; catch them at the boundary layer (API, CLI, etc.).

## Benefits of Custom Exceptions

- **Clarity** — the exception name explains *what went wrong* without reading the message.
- **Precision** — callers can catch exactly the errors they can handle.
- **Encapsulation** — internal implementation details stay hidden behind a stable error API.
- **Richer context** — custom attributes let handlers make decisions, not just log strings.
- **Maintainability** — refactoring error handling is easier when exceptions are well-named and grouped.
