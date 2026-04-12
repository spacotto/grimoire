# Introduction to Pydantic

This document covers Pydantic, a Python library for data validation and settings management. You'll learn what Pydantic is, why it's useful, how to install it, the differences between v1 and v2, its key features, and when and where to use it.

## What is Pydantic?

Pydantic is a Python library that uses **type annotations** to validate and parse data. It ensures that the data your code receives matches the expected types and structure — and raises clear errors when it doesn't.

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
    email: str

user = User(name="Alice", age=30, email="alice@example.com")
print(user)
# name='Alice' age=30 email='alice@example.com'
```

Pydantic models are plain Python classes. Data passed in is **automatically validated and coerced** when possible (e.g., `"30"` → `30`).

## Why Use Pydantic?

- **Validation built-in** — no manual `if` checks for types
- **Clear error messages** — tells you exactly what's wrong and where
- **Auto-coercion** — converts compatible types automatically
- **IDE-friendly** — full autocompletion and type hints support
- **JSON-ready** — serialize/deserialize with one method call
- **Widely adopted** — used by FastAPI, SQLModel, LangChain, and more

## Installing Pydantic

```bash
pip install pydantic
```

For email validation support:

```bash
pip install pydantic[email]
```

Verify the install:

```python
import pydantic
print(pydantic.VERSION)
```

## Pydantic v1 vs. v2

Pydantic v2 (released 2023) was a major rewrite. Key differences:

| Feature | v1 | v2 |
|---|---|---|
| Performance | Slower (pure Python) | ~5–50× faster (Rust core) |
| Validators | `@validator` | `@field_validator` |
| Model config | `class Config` | `model_config = ConfigDict(...)` |
| Serialization | `.dict()`, `.json()` | `.model_dump()`, `.model_dump_json()` |
| Strict mode | Limited | Full strict mode support |

**Use v2 for all new projects.** v1 is still maintained but no longer recommended.

Check your version:

```python
import pydantic
print(pydantic.VERSION)  # e.g., '2.7.1'
```

## Key Pydantic Features

### 1. Type Validation

```python
from pydantic import BaseModel

class Product(BaseModel):
    name: str
    price: float
    in_stock: bool

Product(name="Book", price="12.99", in_stock="yes")
# price coerced to 12.99, in_stock coerced to True
```

### 2. Field Constraints

```python
from pydantic import BaseModel, Field

class Product(BaseModel):
    name: str = Field(min_length=1, max_length=100)
    price: float = Field(gt=0)
    quantity: int = Field(ge=0, default=0)
```

### 3. Nested Models

```python
from pydantic import BaseModel

class Address(BaseModel):
    street: str
    city: str

class User(BaseModel):
    name: str
    address: Address

user = User(name="Alice", address={"street": "123 Main St", "city": "Paris"})
print(user.address.city)  # Paris
```

### 4. Custom Validators

```python
from pydantic import BaseModel, field_validator

class User(BaseModel):
    username: str

    @field_validator("username")
    @classmethod
    def no_spaces(cls, v):
        if " " in v:
            raise ValueError("Username must not contain spaces")
        return v
```

### 5. Serialization

```python
user = User(name="Alice", address={"street": "123 Main St", "city": "Paris"})

user.model_dump()        # → Python dict
user.model_dump_json()   # → JSON string
```

### 6. Optional and Default Values

```python
from pydantic import BaseModel
from typing import Optional

class User(BaseModel):
    name: str
    nickname: Optional[str] = None
    role: str = "viewer"
```

### 7. Strict Mode

```python
from pydantic import BaseModel

class StrictUser(BaseModel):
    model_config = {"strict": True}
    age: int

StrictUser(age="30")  # ❌ ValidationError — no coercion in strict mode
StrictUser(age=30)    # ✅ OK
```

## When to Use Pydantic

**Use Pydantic when you need to:**
- Validate incoming data (APIs, forms, files, environment variables)
- Define clear data contracts between components
- Parse JSON or external data into typed Python objects
- Manage app settings and configuration (via `pydantic-settings`)
- Reduce boilerplate validation code

**You may not need Pydantic if:**
- Your data is entirely internal with no external input
- You're working in a performance-critical loop where overhead matters
- A simple `dataclass` or `TypedDict` is sufficient

## Pydantic in Real-World Applications

### FastAPI (Web APIs)

Pydantic is used natively in FastAPI to validate request bodies and serialize responses:

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float

@app.post("/items/")
def create_item(item: Item):
    return item
```

### Settings Management

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    debug: bool = False

    class Config:
        env_file = ".env"

settings = Settings()
```

### LangChain / AI Pipelines

Pydantic is used to structure LLM outputs, tool inputs, and agent state — ensuring the AI's response conforms to expected schema.

### Data Pipelines / ETL

Parse and validate raw data from CSVs, APIs, or databases before processing:

```python
for row in raw_data:
    try:
        record = MyModel(**row)
    except ValidationError as e:
        log_error(e)
```

*Pydantic is one of the most downloaded Python libraries — with over 300 million monthly downloads — and is a foundational tool in modern Python development.*
