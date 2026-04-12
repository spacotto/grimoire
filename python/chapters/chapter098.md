# Error Handling and ValidationError in Python

Validation is the process of ensuring data meets expected criteria before processing. Python's ecosystem — from standard libraries to frameworks like Pydantic — provides structured tools to catch, communicate, and recover from invalid data. This document covers how `ValidationError` works, how errors are structured, and how to handle them effectively.

## Understanding ValidationError

`ValidationError` is raised when data fails to meet defined constraints. It is most commonly associated with [Pydantic](https://docs.pydantic.dev/), where it signals that one or more fields in a model did not pass validation.

```python
from pydantic import BaseModel, ValidationError

class User(BaseModel):
    name: str
    age: int

try:
    user = User(name="Alice", age="not-a-number")
except ValidationError as e:
    print(e)
```

>[!NOTE]
>Unlike a generic `ValueError`, `ValidationError` aggregates **all** field errors at once rather than stopping at the first failure.

## Error Structure

A `ValidationError` contains a list of error objects. Each error is a dictionary with predictable keys:

| Key | Description |
|-----|-------------|
| `type` | Error type identifier (e.g., `int_parsing`) |
| `loc` | Location of the error (field path as a tuple) |
| `msg` | Human-readable error message |
| `input` | The value that caused the error |
| `ctx` | Optional context with additional details |

```python
try:
    User(name=123, age="bad")
except ValidationError as e:
    for error in e.errors():
        print(error)
```

## Error Messages

Error messages are human-readable strings describing what went wrong. Access them individually or all at once:

```python
except ValidationError as e:
    for error in e.errors():
        print(error["msg"])
    # or
    print(e)  # prints a formatted summary of all errors
```

Messages are consistent and predictable — useful for logging, debugging, or surface-level display.

## Error Locations

The `loc` key identifies **where** in the data the error occurred. For flat models, it's a single-element tuple. For nested models or lists, it reflects the full path:

```python
class Address(BaseModel):
    zip_code: int

class Person(BaseModel):
    address: Address

try:
    Person(address={"zip_code": "abc"})
except ValidationError as e:
    print(e.errors()[0]["loc"])  # ('address', 'zip_code')
```

>[!TIP]
>Use `loc` to map errors back to specific UI fields or API parameters.

## Multiple Validation Errors

Pydantic validates all fields before raising, so a single `ValidationError` may contain multiple errors:

```python
try:
    User(name=None, age="bad")
except ValidationError as e:
    print(e.error_count())   # 2
    print(e.errors())        # list of two error dicts
```

This allows users or clients to see all problems in one response rather than fixing one issue at a time.

## Handling ValidationError

Wrap model instantiation in a `try/except` block to handle errors gracefully:

```python
def create_user(data: dict):
    try:
        return User(**data)
    except ValidationError as e:
        # Log, return, or re-raise depending on context
        return {"errors": e.errors()}
```

>[!TIP]
>Avoid catching broad exceptions like `Exception` when you expect validation errors specifically — it obscures intent and makes debugging harder.

## Custom Error Messages

Override default messages using `Field` with a custom `description`, or use a `@field_validator` to raise targeted errors:

```python
from pydantic import BaseModel, field_validator

class Product(BaseModel):
    price: float

    @field_validator("price")
    @classmethod
    def price_must_be_positive(cls, v):
        if v <= 0:
            raise ValueError("Price must be greater than zero.")
        return v
```

The `ValueError` raised inside a validator is caught by Pydantic and wrapped into a `ValidationError` with your message.

## Error Formatting

For APIs and user-facing systems, format errors into clean, structured output:

```python
def format_errors(e: ValidationError) -> list[dict]:
    return [
        {
            "field": " -> ".join(str(loc) for loc in error["loc"]),
            "message": error["msg"],
            "invalid_value": error.get("input"),
        }
        for error in e.errors()
    ]
```

This makes it straightforward to return structured JSON error responses in REST APIs.

## Debugging Validation Issues

When validation fails unexpectedly:

1. **Print `e.errors()`** — inspect each error dict in full.
2. **Check `input`** — see exactly what value triggered the error.
3. **Check `type`** — the error type string maps to Pydantic's internal error catalog.
4. **Use `model_validate` with `strict=False`** — allows coercion, which can help isolate whether the issue is type mismatch or constraint violation.

```python
User.model_validate({"name": "Alice", "age": "30"}, strict=False)
# Succeeds: "30" is coerced to 30
```

## Error Recovery Strategies

Depending on context, choose an appropriate recovery approach:

**Return errors to the caller** (APIs):
```python
except ValidationError as e:
    return JSONResponse(status_code=422, content={"detail": e.errors()})
```

**Apply defaults or fallbacks** (internal processing):
```python
except ValidationError:
    user = User(name="Anonymous", age=0)
```

**Re-raise with context** (library/service layer):
```python
except ValidationError as e:
    raise RuntimeError(f"Invalid user data received: {e}") from e
```

**Log and skip** (batch processing):
```python
except ValidationError as e:
    logger.warning("Skipping invalid record: %s", e.errors())
    continue
```

>[!TIP]
>Match the strategy to the stakes: user-facing errors need clear messaging; internal failures may warrant strict re-raising.
