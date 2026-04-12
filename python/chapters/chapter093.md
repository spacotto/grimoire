# Type Annotations and Validation in Python 3

Type annotations let you declare the expected types of variables, function parameters, and return values. They don't enforce types at runtime by default — but combined with validation libraries like `pydantic` or runtime checks, they become a powerful tool for writing safer, more readable code.

Python's `typing` module provides the building blocks. `pydantic` handles validation. This document covers both.

## Basic Type Validation

Python's built-in `isinstance()` is the simplest way to validate a type at runtime.

```python
def greet(name: str) -> str:
    if not isinstance(name, str):
        raise TypeError(f"Expected str, got {type(name).__name__}")
    return f"Hello, {name}"

greet("Alice")   # OK
greet(42)        # Raises TypeError
```

For structured validation, `pydantic` models are far more ergonomic:

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int

user = User(name="Alice", age=30)   # OK
user = User(name="Alice", age="x")  # Raises ValidationError
```

## Standard Library Types

Annotate with Python's built-in types directly — no imports needed for simple cases.

```python
x: int = 10
y: float = 3.14
z: str = "hello"
flag: bool = True
data: bytes = b"raw"
```

In function signatures:

```python
def add(a: int, b: int) -> int:
    return a + b

def describe(item: str) -> str:
    return f"Item: {item}"
```

## Optional and Union Types

Use `Optional[T]` when a value can be `T` or `None`. Use `Union[T1, T2]` when multiple types are valid.

```python
from typing import Optional, Union

# Optional[str] is equivalent to Union[str, None]
def find_user(user_id: int) -> Optional[str]:
    return "Alice" if user_id == 1 else None

# Union: accepts int or float
def square(n: Union[int, float]) -> float:
    return n ** 2
```

**Python 3.10+ shorthand:**

```python
def find_user(user_id: int) -> str | None: ...
def square(n: int | float) -> float: ...
```

With `pydantic`:

```python
from pydantic import BaseModel
from typing import Optional

class Profile(BaseModel):
    username: str
    bio: Optional[str] = None  # bio is optional, defaults to None
```

## List and Dict Validation

Annotate collections with their element types.

```python
from typing import List, Dict

scores: List[int] = [10, 20, 30]
config: Dict[str, int] = {"timeout": 30, "retries": 3}
```

**Python 3.9+ shorthand:**

```python
scores: list[int] = [10, 20, 30]
config: dict[str, int] = {"timeout": 30, "retries": 3}
```

With `pydantic`:

```python
from pydantic import BaseModel

class Report(BaseModel):
    tags: list[str]
    metrics: dict[str, float]

Report(tags=["a", "b"], metrics={"score": 9.5})   # OK
Report(tags=[1, 2], metrics={"score": "high"})    # Raises ValidationError
```

## Tuple Validation

Tuples have fixed length and typed positions.

```python
from typing import Tuple

point: Tuple[int, int] = (3, 4)
record: Tuple[str, int, float] = ("Alice", 30, 5.9)
```

Variable-length tuples of one type:

```python
coords: Tuple[float, ...] = (1.0, 2.5, 3.7)
```

With `pydantic`:

```python
from pydantic import BaseModel

class Segment(BaseModel):
    endpoints: tuple[int, int]

Segment(endpoints=(0, 10))    # OK
Segment(endpoints=(0, 1, 2))  # Raises ValidationError (too many elements)
```

## Datetime and Date Validation

`pydantic` handles `datetime` and `date` objects natively, and can parse ISO 8601 strings automatically.

```python
from pydantic import BaseModel
from datetime import datetime, date

class Event(BaseModel):
    name: str
    start_date: date
    created_at: datetime

# Strings are auto-parsed
event = Event(
    name="Launch",
    start_date="2024-06-01",
    created_at="2024-05-15T10:30:00"
)

print(event.start_date)    # datetime.date(2024, 6, 1)
print(event.created_at)   # datetime.datetime(2024, 5, 15, 10, 30)
```

## UUID Validation

`pydantic` accepts `uuid.UUID` objects or valid UUID strings.

```python
from pydantic import BaseModel
from uuid import UUID, uuid4

class Item(BaseModel):
    id: UUID
    name: str

# String is automatically parsed into UUID
item = Item(id="123e4567-e89b-12d3-a456-426614174000", name="Widget")
print(type(item.id))  # <class 'uuid.UUID'>

# Generate a new UUID
item2 = Item(id=uuid4(), name="Gadget")
```

## Email and URL Validation

`pydantic` provides `EmailStr` and `AnyUrl` via `pydantic[email]`.

**Install extras:**

```bash
pip install pydantic[email]
```

```python
from pydantic import BaseModel, EmailStr, AnyUrl

class Contact(BaseModel):
    email: EmailStr
    website: AnyUrl

contact = Contact(
    email="alice@example.com",
    website="https://example.com"
)

Contact(email="not-an-email", website="https://example.com")  # Raises ValidationError
Contact(email="alice@example.com", website="not-a-url")       # Raises ValidationError
```

## Custom Types

Define reusable types with constraints using `Annotated` and field validators.

**Using `Annotated` with constraints:**

```python
from pydantic import BaseModel, Field
from typing import Annotated

PositiveInt = Annotated[int, Field(gt=0)]
ShortStr = Annotated[str, Field(max_length=50)]

class Product(BaseModel):
    name: ShortStr
    quantity: PositiveInt

Product(name="Widget", quantity=5)   # OK
Product(name="Widget", quantity=-1)  # Raises ValidationError
```

**Using `field_validator` for custom logic:**

```python
from pydantic import BaseModel, field_validator

class Username(BaseModel):
    value: str

    @field_validator("value")
    @classmethod
    def no_spaces(cls, v: str) -> str:
        if " " in v:
            raise ValueError("Username must not contain spaces")
        return v.lower()

Username(value="Alice123")   # OK → stored as "alice123"
Username(value="Alice 123")  # Raises ValidationError
```

## Type Coercion

`pydantic` automatically coerces compatible types — e.g., `"42"` into `int`.

```python
from pydantic import BaseModel

class Settings(BaseModel):
    port: int
    debug: bool

s = Settings(port="8080", debug="true")
print(s.port)   # 8080  (int)
print(s.debug)  # True  (bool)
```

**Strict mode** disables coercion when you want exact types only:

```python
from pydantic import BaseModel

class StrictSettings(BaseModel):
    model_config = {"strict": True}
    port: int

StrictSettings(port="8080")  # Raises ValidationError — str is not int
StrictSettings(port=8080)    # OK
```

>[!TIP]
>Use strict mode when incorrect types should be caught immediately rather than silently converted.
