# Advanced Pydantic Features

Pydantic v2 goes far beyond basic validation. This document covers power-user features: configuring model behaviour, controlling serialisation, generating JSON schemas, working with generics, and more. All examples use Python 3.10+ and Pydantic v2.

## Model Configuration

Use `model_config` (a `ConfigDict`) to set model-wide behaviour.

```python
from pydantic import BaseModel
from pydantic.config import ConfigDict

class User(BaseModel):
    model_config = ConfigDict(
        frozen=True,            # instances are immutable
        str_strip_whitespace=True,
        validate_default=True,  # run validators on default values too
        arbitrary_types_allowed=True,
    )

    name: str
    age: int
```

| Option | Description |
| :--- | :--- |
| `frozen` | Makes instances hashable, prevents mutation |
| `populate_by_name` | Accept both field name and alias as input |
| `validate_assignment` | Re-validate on attribute assignment |
| `extra` | See next section |

## Extra Fields Handling

Control what happens when unknown fields are passed in.

```python
from pydantic import BaseModel
from pydantic.config import ConfigDict

class Strict(BaseModel):
    model_config = ConfigDict(extra="forbid")   # raises ValidationError
    x: int

class Loose(BaseModel):
    model_config = ConfigDict(extra="allow")    # stored in __pydantic_extra__
    x: int

class Silent(BaseModel):
    model_config = ConfigDict(extra="ignore")   # default, silently dropped
    x: int

Strict(x=1, y=2)   # ❌ ValidationError
Loose(x=1, y=2)    # ✅ model.__pydantic_extra__ == {"y": 2}
```

## Alias Generators

Automatically derive field aliases from field names — useful for camelCase APIs or snake_case databases.

```python
from pydantic import BaseModel
from pydantic.alias_generators import to_camel
from pydantic.config import ConfigDict

class Response(BaseModel):
    model_config = ConfigDict(
        alias_generator=to_camel,
        populate_by_name=True,  # also accept snake_case input
    )

    user_id: int
    first_name: str

# Accepts camelCase from JSON:
r = Response.model_validate({"userId": 1, "firstName": "Ada"})
print(r.user_id)      # 1
print(r.first_name)   # Ada

# Serialises back to camelCase:
print(r.model_dump(by_alias=True))
# {"userId": 1, "firstName": "Ada"}
```

- Built-in generators: `to_camel`, `to_pascal`, `to_snake`.
- Custom: any `Callable[[str], str]`.

## JSON Schema Generation

Pydantic can generate a JSON Schema automatically.

```python
from pydantic import BaseModel, Field

class Product(BaseModel):
    name: str = Field(description="Product display name")
    price: float = Field(gt=0, description="Price in USD")
    tags: list[str] = Field(default_factory=list)

print(Product.model_json_schema())
# {
#   "title": "Product",
#   "type": "object",
#   "properties": {
#     "name":  {"type": "string",  "description": "Product display name"},
#     "price": {"type": "number",  "description": "Price in USD", "exclusiveMinimum": 0},
#     "tags":  {"type": "array",   "items": {"type": "string"}}
#   },
#   "required": ["name", "price"]
# }
```

Customise the schema title or add metadata via `model_config`:

```python
model_config = ConfigDict(title="My Product Schema", json_schema_extra={"version": "1.0"})
```

## Model Validation Context

Pass arbitrary context into validators at call time — without adding it as a model field.

```python
from pydantic import BaseModel, field_validator, ValidationInfo

class Order(BaseModel):
    item_id: int

    @field_validator("item_id")
    @classmethod
    def item_must_exist(cls, v: int, info: ValidationInfo) -> int:
        allowed = info.context.get("allowed_ids", [])
        if v not in allowed:
            raise ValueError(f"{v} is not a valid item")
        return v

Order.model_validate({"item_id": 42}, context={"allowed_ids": [42, 99]})  # ✅
Order.model_validate({"item_id": 7},  context={"allowed_ids": [42, 99]})  # ❌
```

`info.context` is `None` if no context is passed.

## Strict Mode

By default, Pydantic coerces types (`"3"` → `3`). Strict mode disables this — the input type must match exactly.

```python
from pydantic import BaseModel
from pydantic.config import ConfigDict

# Strict at model level
class StrictModel(BaseModel):
    model_config = ConfigDict(strict=True)
    count: int

StrictModel(count="3")   # ❌ ValidationError — str is not int

# Strict at field level only
from pydantic import Field
from typing import Annotated

class Mixed(BaseModel):
    loose: int
    strict: Annotated[int, Field(strict=True)]

Mixed(loose="3", strict=3)   # ✅
Mixed(loose="3", strict="3") # ❌
```


## Custom JSON Encoders

For types Pydantic doesn't know how to serialise natively, register a custom serialiser.

```python
from pydantic import BaseModel
from pydantic.functional_serializers import PlainSerializer
from typing import Annotated
from decimal import Decimal

# Serialise Decimal as a plain string
DecimalStr = Annotated[
    Decimal,
    PlainSerializer(lambda x: str(x), return_type=str)
]

class Invoice(BaseModel):
    amount: DecimalStr

inv = Invoice(amount=Decimal("9.99"))
print(inv.model_dump())        # {"amount": "9.99"}
print(inv.model_dump_json())   # '{"amount":"9.99"}'
```


## Serialisation Customisation

Fine-grained control over what gets dumped and how.

```python
from pydantic import BaseModel, field_serializer, model_serializer
from datetime import datetime

class Event(BaseModel):
    name: str
    start: datetime

    # Customise a single field's serialisation
    @field_serializer("start")
    def serialize_start(self, value: datetime) -> str:
        return value.strftime("%Y-%m-%d %H:%M")

e = Event(name="PyCon", start=datetime(2025, 5, 15, 9, 0))
print(e.model_dump())
# {"name": "PyCon", "start": "2025-05-15 09:00"}
```

Use `model_serializer` to override the entire dump:

```python
class Envelope(BaseModel):
    payload: dict

    @model_serializer
    def wrap(self) -> dict:
        return {"data": self.payload, "version": 2}
```

Useful `model_dump` options:
- `include` / `exclude`  → set of field names
- `by_alias=True`        → use aliases in output
- `exclude_none=True`    → omit fields that are `None`
- `mode="json"`          → ensure JSON-safe types


## Root Validators

Validate or transform the entire model's data at once. Use `@model_validator` with `mode="before"` (raw input) or `mode="after"` (validated instance).

```python
from pydantic import BaseModel, model_validator
from typing import Self

class DateRange(BaseModel):
    start: int
    end: int

    # Runs after individual fields are validated
    @model_validator(mode="after")
    def check_order(self) -> Self:
        if self.start >= self.end:
            raise ValueError("start must be before end")
        return self

    # Runs before individual fields — receives raw dict
    @model_validator(mode="before")
    @classmethod
    def coerce_input(cls, data: dict) -> dict:
        # Example: accept a tuple as input
        if isinstance(data, (list, tuple)) and len(data) == 2:
            return {"start": data[0], "end": data[1]}
        return data

DateRange(start=1, end=5)     # ✅
DateRange(start=5, end=1)     # ❌ ValueError
DateRange.model_validate([1, 5])  # ✅ coerced by before-validator
```


## Generic Models

Build reusable, type-safe containers with `Generic`.

```python
from pydantic import BaseModel
from typing import Generic, TypeVar

T = TypeVar("T")

class Page(BaseModel, Generic[T]):
    items: list[T]
    total: int
    page: int
    size: int

class UserSummary(BaseModel):
    id: int
    name: str

# Concrete instantiation
user_page = Page[UserSummary].model_validate({
    "items": [{"id": 1, "name": "Ada"}, {"id": 2, "name": "Grace"}],
    "total": 2,
    "page": 1,
    "size": 10,
})

print(user_page.items[0].name)  # Ada

# The schema reflects the concrete type
print(Page[UserSummary].model_json_schema()["title"])  # Page[UserSummary]
```

Generic models work with nested generics, inheritance, and multiple type variables — keeping your API layer DRY and fully validated.
