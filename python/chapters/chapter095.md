# Custom Validation

Pydantic provides a robust validation system that goes beyond simple type checking. Custom validators let you enforce business rules, cross-field constraints, and domain-specific logic — all within the model definition itself.

## Model Validators

Model validators run against the **entire model**, giving you access to all fields at once. Use them when validation logic spans multiple attributes or depends on the model as a whole.

### The `@model_validator` Decorator

```python
from pydantic import BaseModel, model_validator

class Booking(BaseModel):
    check_in: int
    check_out: int

    @model_validator(mode="after")
    def check_dates(self) -> "Booking":
        if self.check_out <= self.check_in:
            raise ValueError("check_out must be after check_in")
        return self
```

### Before vs. After Mode

| Mode | Input | Use when |
|------|-------|----------|
| `"before"` | Raw, unvalidated data (`dict`) | Normalising input before type coercion |
| `"after"` | Fully validated model instance | Checking relationships between parsed fields |

```python
@model_validator(mode="before")
@classmethod
def normalise_input(cls, data: dict) -> dict:
    # Runs before field validation
    data["name"] = data.get("name", "").strip()
    return data
```

## Field Validators (Deprecated in v2)

>[!NOTE]
>`@validator` is deprecated in Pydantic v2. Use `@field_validator` instead.

```python
from pydantic import BaseModel, field_validator

class User(BaseModel):
    username: str

    @field_validator("username")
    @classmethod
    def no_spaces(cls, v: str) -> str:
        if " " in v:
            raise ValueError("username cannot contain spaces")
        return v.lower()
```

## Multi-Field Validation

Validate several fields with a single validator by passing multiple field names.

```python
class Address(BaseModel):
    city: str
    country: str

    @field_validator("city", "country")
    @classmethod
    def not_empty(cls, v: str) -> str:
        if not v.strip():
            raise ValueError("field cannot be empty")
        return v
```

## Conditional Validation

Apply rules only when certain conditions are met.

```python
class Payment(BaseModel):
    method: str          # "card" or "bank"
    card_number: str | None = None

    @model_validator(mode="after")
    def require_card_number(self) -> "Payment":
        if self.method == "card" and not self.card_number:
            raise ValueError("card_number is required when method is 'card'")
        return self
```

## Cross-Field Dependencies

Use `@model_validator(mode="after")` to express rules where one field constrains another.

```python
class Range(BaseModel):
    minimum: float
    maximum: float

    @model_validator(mode="after")
    def min_below_max(self) -> "Range":
        if self.minimum >= self.maximum:
            raise ValueError("minimum must be less than maximum")
        return self
```

## Business Logic Validation

Embed domain rules directly in the model to keep constraints close to the data they protect.

```python
class Order(BaseModel):
    quantity: int
    unit_price: float
    discount: float = 0.0

    @model_validator(mode="after")
    def validate_discount(self) -> "Order":
        total = self.quantity * self.unit_price
        if self.discount > total:
            raise ValueError("discount cannot exceed order total")
        return self
```

## Validation Error Messages

### Raising `ValidationError`

Pydantic raises `ValidationError` automatically when a validator throws `ValueError` or `AssertionError`. You can catch and inspect it:

```python
from pydantic import ValidationError

try:
    Booking(check_in=10, check_out=5)
except ValidationError as e:
    print(e)
    # 1 validation error for Booking
    # check_out
    #   Value error, check_out must be after check_in [type=value_error, ...]
```

To surface multiple errors at once, raise a list of `InitErrorDetails` — or let each field validator raise independently so Pydantic collects them all before surfacing the result.

```python
from pydantic_core import InitErrorDetails, PydanticCustomError

raise PydanticCustomError(
    "invalid_range",
    "minimum {min} must be less than maximum {max}",
    {"min": self.minimum, "max": self.maximum},
)
```

>[!TIP]
>Use `PydanticCustomError` when you need a stable error **type** string for programmatic handling downstream.
