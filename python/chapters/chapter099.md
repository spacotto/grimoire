# Pydantic Best Practices

Pydantic is Python's most widely used data validation library. It uses type annotations to define data schemas and validates data at runtime — catching errors early, reducing boilerplate, and making your code self-documenting.

This guide covers practical best practices for using Pydantic v2 effectively, from model design to production deployment.

## Model Design Principles

Keep models focused and explicit.

```python
from pydantic import BaseModel, Field
from typing import Optional
from datetime import datetime

# Good: clear, typed, constrained fields
class User(BaseModel):
    id: int
    username: str = Field(min_length=3, max_length=50)
    email: str
    created_at: datetime = Field(default_factory=datetime.utcnow)
    bio: Optional[str] = None

# Avoid: untyped or overly permissive fields
class BadUser(BaseModel):
    data: dict  # too vague — what's inside?
```

**Principles to follow:**

- Always annotate every field with a specific type
- Use `Field(...)` to add constraints, defaults, and descriptions
- Prefer `Optional[X]` over `X | None` for clarity (both work in v2)
- Use `model_config` instead of the inner `class Config` (v2 style)

```python
from pydantic import BaseModel, ConfigDict

class StrictUser(BaseModel):
    model_config = ConfigDict(strict=True, frozen=True)

    id: int
    name: str
```

## Validation Performance

Pydantic v2 is built on Rust (`pydantic-core`) and is significantly faster than v1. Still, a few habits help.

```python
from pydantic import BaseModel, field_validator, model_validator
from typing import Self

class Order(BaseModel):
    quantity: int
    unit_price: float
    total: float

    @model_validator(mode="after")
    def check_total(self) -> Self:
        expected = self.quantity * self.unit_price
        if abs(self.total - expected) > 0.01:
            raise ValueError(f"Total mismatch: expected {expected}, got {self.total}")
        return self
```

**Performance tips:**

- Use `model_validate()` instead of `**dict` unpacking for large datasets
- Use `TypeAdapter` for validating non-model types (lists, primitives) without a full model
- Avoid redundant validators — let Pydantic's type system do the heavy lifting

```python
from pydantic import TypeAdapter

adapter = TypeAdapter(list[int])
adapter.validate_python([1, 2, "3"])  # raises on "3" if strict
```

## Model Organisation

Structure models to reflect your domain, not your database tables.

```python
# Separate concerns: input vs output vs stored form
class UserCreate(BaseModel):
    username: str
    email: str
    password: str  # raw input only

class UserResponse(BaseModel):
    id: int
    username: str
    email: str
    # password deliberately excluded

class UserInDB(UserResponse):
    hashed_password: str
```

**Organisation patterns:**

- Group related models in a dedicated `schemas/` or `models/` module
- Use inheritance to share common fields (`BaseTimestampModel`, `BaseAuditModel`)
- Keep request/response models separate from internal/DB models

```python
from datetime import datetime

class TimestampMixin(BaseModel):
    created_at: datetime = Field(default_factory=datetime.utcnow)
    updated_at: Optional[datetime] = None

class Article(TimestampMixin):
    title: str
    content: str
```

## Documentation with Models

Pydantic models are self-documenting when you use `Field(description=...)`.

```python
class Product(BaseModel):
    """Represents a purchasable product in the catalogue."""

    id: int = Field(description="Unique product identifier")
    name: str = Field(min_length=1, max_length=200, description="Display name")
    price: float = Field(gt=0, description="Price in USD, must be positive")
    tags: list[str] = Field(default_factory=list, description="Searchable tags")
```

Generate a JSON schema directly:

```python
import json
print(json.dumps(Product.model_json_schema(), indent=2))
```

This integrates automatically with FastAPI's OpenAPI docs and tools like Swagger.

## Testing Pydantic Models

Test validation logic explicitly — both valid and invalid inputs.

```python
import pytest
from pydantic import ValidationError

class Rectangle(BaseModel):
    width: float = Field(gt=0)
    height: float = Field(gt=0)

def test_valid_rectangle():
    r = Rectangle(width=10.0, height=5.0)
    assert r.width == 10.0

def test_invalid_width():
    with pytest.raises(ValidationError) as exc_info:
        Rectangle(width=-1, height=5.0)
    errors = exc_info.value.errors()
    assert any(e["loc"] == ("width",) for e in errors)

def test_missing_field():
    with pytest.raises(ValidationError):
        Rectangle(width=10.0)  # height missing
```

**Testing checklist:**

- Test boundary values (e.g., `gt=0` means `0` should fail, `0.001` should pass)
- Test that extra fields are rejected (if `extra="forbid"` is set)
- Test serialisation with `.model_dump()` and `.model_dump_json()`

## Avoiding Common Pitfalls

### 1. Mutable defaults

```python
# Bad: shared mutable default
class Bad(BaseModel):
    items: list = []  # same list object across all instances!

# Good: use default_factory
class Good(BaseModel):
    items: list[str] = Field(default_factory=list)
```

### 2. Forgetting `model_dump()` vs `dict()`

```python
user = User(id=1, username="alice", email="alice@example.com")

# v2: use model_dump()
data = user.model_dump()

# Deprecated in v2 (still works but avoid):
data = user.dict()
```

### 3. Validators that silently pass

```python
from pydantic import field_validator

class Event(BaseModel):
    name: str

    @field_validator("name")
    @classmethod
    def name_must_not_be_empty(cls, v: str) -> str:
        if not v.strip():
            raise ValueError("name cannot be blank")
        return v.strip()  # always return the value!
```

### 4. Using `Any` type

Avoid `Any` — it disables validation entirely. Use `Union` types or more specific constraints instead.

## Migration from v1 to v2

Pydantic v2 introduced breaking changes. Key differences:

| v1 | v2 |
|---|---|
| `class Config` | `model_config = ConfigDict(...)` |
| `.dict()` | `.model_dump()` |
| `.json()` | `.model_dump_json()` |
| `.parse_obj()` | `.model_validate()` |
| `@validator` | `@field_validator` |
| `@root_validator` | `@model_validator` |

```python
# v1
from pydantic import validator

class Old(BaseModel):
    name: str

    @validator("name")
    def check_name(cls, v):
        return v.strip()

# v2
from pydantic import field_validator

class New(BaseModel):
    name: str

    @field_validator("name")
    @classmethod
    def check_name(cls, v: str) -> str:
        return v.strip()
```

Use the official migration tool to find deprecated usage:

```bash
pip install bump-pydantic
bump-pydantic .
```

## Integration with FastAPI

Pydantic and FastAPI are designed to work together. FastAPI uses Pydantic models for request bodies, query params, and responses.

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel, Field

app = FastAPI()

class ItemCreate(BaseModel):
    name: str = Field(min_length=1)
    price: float = Field(gt=0)

class ItemResponse(BaseModel):
    id: int
    name: str
    price: float

@app.post("/items", response_model=ItemResponse)
async def create_item(item: ItemCreate) -> ItemResponse:
    # FastAPI validates the request body automatically
    saved = save_to_db(item)  # your DB logic
    return ItemResponse(id=saved.id, name=item.name, price=item.price)
```

**FastAPI integration tips:**

- Always use `response_model` to control what gets serialised back to the client
- Use separate `Create`, `Update`, and `Response` schemas per resource
- Leverage `model_json_schema()` for custom OpenAPI documentation

## Database Integration

Pydantic works well with ORMs via compatibility modes.

### With SQLAlchemy (using `from_orm` / `model_validate`)

```python
from pydantic import BaseModel, ConfigDict
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import DeclarativeBase

class Base(DeclarativeBase):
    pass

class UserORM(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    name = Column(String)
    email = Column(String)

# Pydantic schema with ORM mode enabled
class UserSchema(BaseModel):
    model_config = ConfigDict(from_attributes=True)

    id: int
    name: str
    email: str

# Convert ORM object → Pydantic model
user_orm = session.get(UserORM, 1)
user_schema = UserSchema.model_validate(user_orm)
```

### With SQLModel (combines SQLAlchemy + Pydantic)

```python
from sqlmodel import SQLModel, Field

class User(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    name: str
    email: str
```

SQLModel is ideal when you want one schema for both DB and validation — but keep it simple; large projects often benefit from keeping schemas separate.

## Production Considerations

### Environment configuration

Use Pydantic's `BaseSettings` for typed, validated environment config:

```python
from pydantic_settings import BaseSettings  # pip install pydantic-settings

class Settings(BaseSettings):
    database_url: str
    api_key: str
    debug: bool = False
    max_connections: int = 10

    model_config = ConfigDict(env_file=".env")

settings = Settings()  # reads from environment or .env file
```

### Serialisation at scale

```python
# Exclude unset fields from API responses (avoids sending null for everything)
user.model_dump(exclude_unset=True)

# Exclude sensitive fields
user.model_dump(exclude={"password", "hashed_password"})

# Include only specific fields
user.model_dump(include={"id", "username"})
```

### Error handling in APIs

```python
from fastapi import Request
from fastapi.responses import JSONResponse
from pydantic import ValidationError

@app.exception_handler(ValidationError)
async def validation_error_handler(request: Request, exc: ValidationError):
    return JSONResponse(
        status_code=422,
        content={"detail": exc.errors()}
    )
```

### Key production habits

- Pin your Pydantic version (`pydantic>=2.5,<3.0`) to avoid unexpected breakage
- Use `model_config = ConfigDict(frozen=True)` for immutable response models
- Avoid `model_rebuild()` in hot paths — it's expensive
- Profile validation with `timeit` or `py-spy` if you're processing thousands of records per second
