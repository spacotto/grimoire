# Raising Exceptions in Python 3
Raising exceptions is how you signal that something has gone wrong in your code. Python lets you raise built-in or custom exceptions at any point, giving you precise control over error handling and program flow.

## The `raise` Keyword
Use `raise` to trigger an exception manually.
```python
raise ValueError("Something went wrong")
```

You can raise:
- An exception class: `raise ValueError`
- An exception instance: `raise ValueError("message")`

---

## When to Raise Exceptions
- Invalid input or arguments
- Unsupported operations
- Failed preconditions or postconditions
- Enforcing contracts in APIs or libraries

## Raising Built-in Exceptions
Use the most semantically appropriate built-in exception.
```python
def divide(a, b):
    if b == 0:
        raise ZeroDivisionError("Denominator cannot be zero.")
    return a / b

def get_user(user_id):
    if not isinstance(user_id, int):
        raise TypeError(f"Expected int, got {type(user_id).__name__}")
```

>[!TIP]
>Common built-ins: `ValueError`, `TypeError`, `KeyError`, `IndexError`, `AttributeError`, `RuntimeError`, `NotImplementedError`.

---

## Raising Custom Exceptions
Subclass `Exception` (or a built-in) to create domain-specific errors.
```python
class InsufficientFundsError(Exception):
    pass

class AuthenticationError(PermissionError):
    pass

def withdraw(amount, balance):
    if amount > balance:
        raise InsufficientFundsError(f"Cannot withdraw {amount}, balance is {balance}.")
```

Group related exceptions under a base class for easier handling:
```python
class AppError(Exception):
    pass

class DatabaseError(AppError):
    pass

class NetworkError(AppError):
    pass
```

---

## Creating Helpful Error Messages

A good error message answers: *what went wrong*, *what was expected*, *what was received*.
```python
def set_age(age):
    if not isinstance(age, int):
        raise TypeError(f"Age must be an int, got {type(age).__name__!r}.")
    if age < 0 or age > 150:
        raise ValueError(f"Age must be between 0 and 150, got {age}.")
```

You can also attach extra context via custom attributes:
```python
class ValidationError(Exception):
    def __init__(self, message, field=None):
        super().__init__(message)
        self.field = field

raise ValidationError("Value is required.", field="email")
```

---

## Re-raising Exceptions

Use a bare `raise` inside an `except` block to re-raise the current exception without losing the traceback.
```python
try:
    risky_operation()
except ValueError:
    log_error()
    raise  # re-raises the original ValueError
```

## Exception Chaining
Use `raise ... from ...` to chain exceptions, preserving the original cause.
```python
try:
    value = int(user_input)
except ValueError as e:
    raise RuntimeError("Failed to process input.") from e
```

This sets `__cause__` and shows both exceptions in the traceback.

To suppress the original context (hide it from the traceback):
```python
raise RuntimeError("Something failed.") from None
```

## Input Validation with Exceptions
A common pattern is a dedicated validation function that raises on bad input.
```python
def validate_username(username):
    if not isinstance(username, str):
        raise TypeError("Username must be a string.")
    if len(username) < 3:
        raise ValueError("Username must be at least 3 characters.")
    if not username.isalnum():
        raise ValueError("Username must be alphanumeric.")
    return username
```

For multiple validations, collect errors before raising:
```python
def validate_config(config):
    errors = []
    if "host" not in config:
        errors.append("Missing 'host'.")
    if "port" not in config:
        errors.append("Missing 'port'.")
    if errors:
        raise ValueError("Invalid config:\n" + "\n".join(errors))
```
