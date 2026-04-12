# Field Validation in Pydantic

Pydantic's field validation system provides a declarative way to enforce data integrity in Python applications. By defining constraints directly on model fields, you get automatic validation, clear error messages, and self-documenting schemas — all without writing boilerplate validation logic.

## The `Field` Function

`Field()` is Pydantic's primary tool for annotating model attributes with validation rules, defaults, and metadata.

```python
from pydantic import BaseModel, Field

class Product(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)
    price: float = Field(..., gt=0)
```

>[!IMPORTANT]
>`...` (Ellipsis) marks a field as **required** — Pydantic will raise a `ValidationError` if it's missing.

## Field Constraints

### String Constraints

| Parameter    | Description                          |
|-------------|--------------------------------------|
| `min_length` | Minimum number of characters         |
| `max_length` | Maximum number of characters         |
| `pattern`    | Regex the string must fully match    |

```python
class User(BaseModel):
    username: str = Field(..., min_length=3, max_length=20)
    email: str = Field(..., pattern=r'^[\w.-]+@[\w.-]+\.\w+$')
```

### Numeric Constraints

| Parameter | Description              |
|-----------|--------------------------|
| `ge`      | Greater than or equal to |
| `le`      | Less than or equal to    |
| `gt`      | Strictly greater than    |
| `lt`      | Strictly less than       |

```python
class Measurement(BaseModel):
    temperature: float = Field(..., ge=-273.15)   # absolute zero lower bound
    percentage: float = Field(..., ge=0, le=100)
    rank: int = Field(..., gt=0, lt=1000)
```

## Default Values

Fields can have a static default or a factory that generates a new value each time.

```python
from datetime import datetime

class Event(BaseModel):
    status: str = Field(default="pending")
    created_at: datetime = Field(default_factory=datetime.utcnow)
```

>[!TIP]
>Use `default_factory` for mutable defaults like lists or dicts — never `default=[]`.

## Required vs. Optional Fields

```python
from typing import Optional

class Profile(BaseModel):
    name: str                              # required, no default
    bio: Optional[str] = None             # optional, defaults to None
    age: Optional[int] = Field(None, ge=0)  # optional with constraint
```

>[!WARNING]
>A field is **required** if it has no default. An `Optional` field typed with `None` default is always valid even when omitted.

## Field Descriptions and Metadata

Attach human-readable documentation directly to fields. This surfaces in generated JSON schemas and API docs.

```python
class Article(BaseModel):
    title: str = Field(..., description="The article headline, max 200 chars", max_length=200)
    word_count: int = Field(..., description="Total word count", ge=1)
```

## Field Aliases

Aliases let you accept different key names from external data while keeping clean Python attribute names.

```python
class Record(BaseModel):
    model_config = {"populate_by_name": True}

    record_id: int = Field(..., alias="recordId")
    created_by: str = Field(..., alias="createdBy")

# Parses both:
Record.model_validate({"recordId": 1, "createdBy": "alice"})
Record(record_id=1, created_by="alice")   # works because populate_by_name=True
```

## Field Examples

Examples annotate fields with sample values, which appear in the generated JSON schema.

```python
class Address(BaseModel):
    street: str = Field(..., examples=["10 Downing Street", "1600 Pennsylvania Ave"])
    zip_code: str = Field(..., examples=["75001", "10001"])
```

## Computed Fields

`@computed_field` exposes a `@property` as a validated, serialisable model field — no duplication of data needed.

```python
from pydantic import computed_field

class Rectangle(BaseModel):
    width: float = Field(..., gt=0)
    height: float = Field(..., gt=0)

    @computed_field
    @property
    def area(self) -> float:
        return self.width * self.height
```

`area` is read-only, always consistent, and included in `.model_dump()` and `.model_json_schema()` output.
