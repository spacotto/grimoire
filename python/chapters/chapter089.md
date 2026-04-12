# Introduction to Data Validation

Data validation ensures that the data your program receives is correct, complete, and safe to use. Python's type hints help at development time, but they don't protect you at runtime — that's where **Pydantic** comes in. Pydantic is a Python library that validates data automatically using the type annotations you already write. This document covers why validation matters, what problems it solves, and how Pydantic compares to doing it by hand.

## Why Data Validation Matters

Programs receive data from many sources: HTTP requests, config files, databases, message queues, user input. You can never fully trust external data.

Without validation, bad data causes:

- **Crashes** — a `None` where an `int` was expected
- **Silent corruption** — a string `"true"` stored instead of a boolean `True`
- **Security issues** — malicious payloads slipping through unguarded

Validating data at the boundary (the point where it enters your system) catches problems early and makes errors easier to diagnose.

## Validation vs. Type Checking

These are two different things — easy to confuse, important to distinguish.

| | Type Checking | Data Validation |
|---|---|---|
| **When** | Before runtime (static) | At runtime (dynamic) |
| **Tool** | `mypy`, `pyright`, IDE | Pydantic, `marshmallow`, custom code |
| **Checks** | Code correctness | Actual data values |
| **Catches** | Wrong function call signatures | A user sending `"abc"` for an `int` field |

```python
def greet(name: str) -> str:
    return f"Hello, {name}"

greet(123)  # mypy warns here — but Python still runs it at runtime
```

Type hints alone won't stop `123` from being passed at runtime. Validation will.

## Runtime Validation

Runtime validation means checking data **while your program runs**, not before.

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int

user = User(name="Alice", age="30")  # "30" is a string — Pydantic coerces it to int
print(user.age)  # 30 (int)

user = User(name="Alice", age="thirty")  # Raises ValidationError — can't coerce
```

Pydantic validates and coerces data on instantiation. If the data doesn't fit, it raises a clear `ValidationError` with details about what failed and why.

## Data Integrity and Quality

Validation isn't just about types — it's about **meaning**.

```python
from pydantic import BaseModel, field_validator

class Product(BaseModel):
    name: str
    price: float
    stock: int

    @field_validator("price")
    @classmethod
    def price_must_be_positive(cls, v):
        if v <= 0:
            raise ValueError("price must be greater than zero")
        return v

    @field_validator("name")
    @classmethod
    def name_must_not_be_empty(cls, v):
        if not v.strip():
            raise ValueError("name cannot be blank")
        return v
```

A `price` of `-5.0` is a valid `float`, but it's not a valid price. Custom validators let you encode business rules directly into your data models.

## Validation in APIs and Services

Pydantic is most widely used in **FastAPI**, where it handles request and response validation automatically.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    quantity: int
    price: float

@app.post("/items")
def create_item(item: Item):
    return {"message": f"Created {item.name}"}
```

When a client sends a POST request:
- FastAPI uses Pydantic to parse the JSON body
- Invalid data returns a `422 Unprocessable Entity` with a detailed error message automatically
- Valid data is passed to your function as a typed `Item` object

This removes boilerplate and keeps your route handlers focused on logic, not input parsing.

## Common Validation Challenges

### 1. Optional and missing fields

```python
from pydantic import BaseModel
from typing import Optional

class Profile(BaseModel):
    username: str
    bio: Optional[str] = None  # Optional with a default
```

Fields without defaults are required. Pydantic raises an error if they're missing.

### 2. Nested models

```python
class Address(BaseModel):
    street: str
    city: str

class Person(BaseModel):
    name: str
    address: Address  # Nested — validated recursively

data = {"name": "Bob", "address": {"street": "12 Elm St", "city": "Lyon"}}
person = Person(**data)
```

Pydantic validates nested structures automatically.

### 3. Lists and collections

```python
from pydantic import BaseModel
from typing import List

class Order(BaseModel):
    items: List[str]
    quantities: List[int]
```

Each element in the list is validated against the declared type.

### 4. Unknown fields

By default, Pydantic ignores extra fields. You can change this:

```python
from pydantic import BaseModel

class Strict(BaseModel):
    model_config = {"extra": "forbid"}  # Raises error on unexpected fields

    name: str
```

## Manual Validation vs. Libraries

### Manual validation

```python
def parse_user(data: dict):
    if not isinstance(data.get("name"), str):
        raise ValueError("name must be a string")
    if not isinstance(data.get("age"), int) or data["age"] < 0:
        raise ValueError("age must be a non-negative integer")
    return data
```

This works, but it grows fast. Every field needs its own checks. Error messages are inconsistent. Nested structures multiply the problem.

### With Pydantic

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
```

Two lines. Pydantic handles:
- Type coercion (`"25"` → `25`)
- Missing field detection
- Clear, structured error messages
- JSON serialization (`.model_dump()`, `.model_dump_json()`)

### When to use each

| Scenario | Approach |
|---|---|
| Simple script, no external input | Type hints alone may suffice |
| External data, APIs, config files | Use Pydantic |
| Highly custom rules, legacy systems | Combine Pydantic + custom validators |
| Performance-critical, no dependencies | Manual validation (rare) |

> **Key takeaway:** Type hints describe intent. Pydantic enforces it. For any Python project that handles external data, Pydantic reduces bugs, removes boilerplate, and makes your data contracts explicit.
