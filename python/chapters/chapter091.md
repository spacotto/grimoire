# BaseModel Fundamentals

Pydantic's `BaseModel` is the foundation for data validation in Python. By defining models as classes with typed fields, Pydantic automatically validates input, converts types, and serialises data — eliminating boilerplate and reducing bugs at the boundary between your code and the outside world.

## The BaseModel Class

`BaseModel` lives in the `pydantic` module. Every model you create inherits from it.

```python
from pydantic import BaseModel
```

Under the hood, `BaseModel` uses Python type annotations to build a validation schema. When you instantiate a model, Pydantic checks that every value matches its declared type before the object is created.

## Creating Your First Model

Define a model as a plain Python class. Use standard type annotations for fields.

```python
from pydantic import BaseModel

class User(BaseModel):
    id: int
    name: str
    email: str
    is_active: bool = True  # field with a default value
```

- Fields **without** defaults are required.
- Fields **with** defaults are optional.

## Model Instantiation

Pass field values as keyword arguments.

```python
user = User(id=1, name="Alice", email="alice@example.com")
print(user)
# id=1 name='Alice' email='alice@example.com' is_active=True
```

If a required field is missing or a value fails validation, Pydantic raises a `ValidationError` immediately.

```python
from pydantic import ValidationError

try:
    bad_user = User(id="not-an-int", name="Bob", email="bob@example.com")
except ValidationError as e:
    print(e)
```

## Automatic Type Conversion

Pydantic coerces values when it can do so safely.

```python
user = User(id="42", name="Alice", email="alice@example.com")
print(user.id)        # 42  (str → int)
print(type(user.id))  # <class 'int'>
```

The string `"42"` is silently converted to the integer `42`. If conversion is impossible (e.g. `"abc"` → `int`), validation fails.

## Accessing Model Fields

Fields are plain Python attributes.

```python
print(user.name)      # Alice
print(user.is_active) # True
```

You can also access all field names via the model's schema:

```python
print(User.model_fields.keys())
# dict_keys(['id', 'name', 'email', 'is_active'])
```

## Model Serialisation

### `model_dump()` and `model_dump_json()`

Convert a model instance to a plain dictionary or a JSON string.

```python
user = User(id=1, name="Alice", email="alice@example.com")

# Dictionary
data = user.model_dump()
print(data)
# {'id': 1, 'name': 'Alice', 'email': 'alice@example.com', 'is_active': True}

# JSON string
json_data = user.model_dump_json()
print(json_data)
# '{"id":1,"name":"Alice","email":"alice@example.com","is_active":true}'
```

Both methods accept options to include or exclude specific fields:

```python
user.model_dump(exclude={"email"})
# {'id': 1, 'name': 'Alice', 'is_active': True}

user.model_dump(include={"id", "name"})
# {'id': 1, 'name': 'Alice'}
```

## Model Comparison

Two model instances are equal if they have the same type and field values.

```python
user1 = User(id=1, name="Alice", email="alice@example.com")
user2 = User(id=1, name="Alice", email="alice@example.com")
user3 = User(id=2, name="Bob",   email="bob@example.com")

print(user1 == user2)  # True
print(user1 == user3)  # False
```

Comparison is value-based, not identity-based — two separate objects with identical data are considered equal.

## Model Copying

`model_copy()` creates a new instance, optionally overriding specific fields.

```python
updated_user = user1.model_copy(update={"name": "Alicia"})

print(updated_user.name)  # Alicia
print(user1.name)         # Alice  ← original unchanged
```

This is the idiomatic way to produce a modified version of a model without mutating the original.

## Model Immutability

By default, Pydantic models are **mutable** — you can reassign field values after creation.

```python
user.name = "Bob"  # allowed by default
```

To enforce immutability, set `frozen=True` in the model config:

```python
class ImmutableUser(BaseModel):
    model_config = {"frozen": True}

    id: int
    name: str

u = ImmutableUser(id=1, name="Alice")
u.name = "Bob"  # raises ValidationError: Instance is frozen
```

Frozen models are also **hashable**, which makes them usable as dictionary keys or in sets.

```python
user_set = {ImmutableUser(id=1, name="Alice")}
```
