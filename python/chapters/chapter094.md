# Enums and Literal Types in Pydantic

Pydantic supports Python's `Enum` and `Literal` types for defining fields that accept a restricted set of values. These provide both validation and clear documentation of expected inputs.

## Python Enum with Pydantic

Pydantic natively integrates with Python's built-in `enum.Enum`. When a model field is annotated with an Enum type, Pydantic validates that the input matches one of the defined enum members.

```python
from enum import Enum
import pydantic

class Color(Enum):
    RED = "red"
    GREEN = "green"
    BLUE = "blue"
```

## Defining Enums for Models

Use enums as field types directly in a model definition:

```python
from pydantic import BaseModel

class Palette(BaseModel):
    color: Color

palette = Palette(color=Color.RED)       # ✓ valid
palette = Palette(color="red")           # ✓ also valid (Pydantic coerces the string)
palette = Palette(color="yellow")        # ✗ raises ValidationError
```

>[!NOTE]
>Pydantic accepts both the enum member itself and its value.

## String Enums

For string-valued enums, you can also subclass `str` and `Enum`. This is useful when you want the enum to behave like a string (e.g., for serialisation or comparison purposes).

```python
from enum import Enum

class Direction(str, Enum):
    NORTH = "north"
    SOUTH = "south"
    EAST = "east"
    WEST = "west"
```

With `str, Enum`:
- Instances behave like strings.
- `Direction.NORTH == "north"` evaluates to `True`.
- JSON serialisation returns the string value directly (no need for `.value`).

## Integer Enums

Similarly, you can use `int, Enum` for integer-valued enums:

```python
from enum import Enum

class Priority(int, Enum):
    LOW = 1
    MEDIUM = 2
    HIGH = 3
```

With `int, Enum`:
- `Priority.HIGH == 3` evaluates to `True`.
- Useful for ordered comparisons: `Priority.HIGH > Priority.LOW`.

## Literal Types

`Literal` (from `typing`) is a lightweight alternative when you want to restrict a field to specific values without defining a full Enum class.

```python
from typing import Literal
from pydantic import BaseModel

class TrafficLight(BaseModel):
    status: Literal["red", "yellow", "green"]

TrafficLight(status="red")     # ✓ valid
TrafficLight(status="blue")    # ✗ raises ValidationError
```

`Literal` also works with integers and mixed types:

```python
class Config(BaseModel):
    mode: Literal[1, 2, 3]
    flag: Literal[True]
```

## Enum Validation

Pydantic validates enum fields by:
1. Checking if the input is an existing enum member.
2. If not, attempting to coerce the input to a matching enum value.
3. Raising a `ValidationError` if no match is found.

```python
class Status(str, Enum):
    ACTIVE = "active"
    INACTIVE = "inactive"

class User(BaseModel):
    status: Status

User(status="active")          # ✓ coerced to Status.ACTIVE
User(status=Status.ACTIVE)     # ✓ passed directly
User(status="pending")         # ✗ ValidationError
```

## When to Use Enums vs. Literals

| Situation | Recommendation |
|-----------|----------------|
| Values reused across multiple models | Use `Enum` |
| Need string/int-like behaviour | Use `str, Enum` or `int, Enum` |
| One-off field with a fixed set of values | Use `Literal` |
| Need iteration or `.value` access programmatically | Use `Enum` |
| Simple, readable constraints | Use `Literal` |

>[!TIP]
>**Rule of thumb**: Use `Literal` for simplicity; use `Enum` when the type carries meaning beyond the field.

## Enum Serialisation

By default, Pydantic serialises enum fields using the **enum value** (not the member name).

```python
class Color(str, Enum):
    RED = "red"

class Palette(BaseModel):
    color: Color

p = Palette(color=Color.RED)
print(p.model_dump())          # {'color': 'red'}
print(p.model_dump_json())     # '{"color":"red"}'
```

To serialise by **name** instead:

```python
print(p.model_dump(mode="python"))  # {'color': <Color.RED: 'red'>}
```

Or use `use_enum_values=True` in the model config to store values instead of enum members internally:

```python
class Palette(BaseModel):
    model_config = {"use_enum_values": True}
    color: Color
```
