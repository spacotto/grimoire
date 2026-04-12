# Nested Models in Pydantic

Pydantic lets you embed one model inside another — called **nested models**. This is how you represent structured, hierarchical data: addresses inside users, items inside orders, nodes inside trees. Pydantic validates each level automatically, so your entire data structure is type-safe from top to bottom.

## Model Composition

Build complex models by composing simpler ones. Instead of cramming every field into a single flat model, group related fields into their own models and embed them.

```python
from pydantic import BaseModel

class Address(BaseModel):
    street: str
    city: str
    zip_code: str

class User(BaseModel):
    name: str
    address: Address
```

`Address` is a first-class model. `User` composes it. Each model stays focused and reusable.

## Nested Model Definition

A nested model is a model used as a **field type** inside another model. Pydantic handles coercion automatically — pass a plain dict and it converts it.

```python
user = User(
    name="Alice",
    address={"street": "123 Main St", "city": "Paris", "zip_code": "75001"}
)

print(user.address.city)   # Paris
print(type(user.address))  # <class '__main__.Address'>
```

The dict becomes an `Address` instance. Access nested fields with dot notation.

## One-to-One Relationships

One model field holds exactly one instance of another model.

```python
class Engine(BaseModel):
    horsepower: int
    fuel_type: str

class Car(BaseModel):
    brand: str
    engine: Engine
```

One `Car` → one `Engine`. Clean, direct, validated.

## One-to-Many Relationships

Use `list` to hold multiple instances of a nested model.

```python
class OrderItem(BaseModel):
    product: str
    quantity: int

class Order(BaseModel):
    order_id: int
    items: list[OrderItem]
```

One `Order` → many `OrderItem`s. Pydantic validates each item in the list.

```python
order = Order(
    order_id=1,
    items=[
        {"product": "Book", "quantity": 2},
        {"product": "Pen",  "quantity": 5},
    ]
)
```

## Lists of Models

Need to validate a standalone list of models — not wrapped in a parent? Use `TypeAdapter`.

```python
from pydantic import TypeAdapter

adapter = TypeAdapter(list[OrderItem])

items = adapter.validate_python([
    {"product": "Book", "quantity": 2},
    {"product": "Pen",  "quantity": 5},
])
```

Useful for validating API payloads or collections without a wrapper model.

## Nested Model Validation

Pydantic validates nested models **recursively**. Every field at every level is checked.

```python
from pydantic import BaseModel, Field

class Product(BaseModel):
    name: str
    price: float = Field(gt=0)

class Cart(BaseModel):
    products: list[Product]

# Raises ValidationError: price must be > 0
cart = Cart(products=[{"name": "Laptop", "price": -100}])
```

Constraints on nested models behave exactly like constraints on top-level fields.

## Deep Nesting Considerations

You can nest models multiple levels deep. Do it when it reflects your data structure, not for its own sake.

```python
class Room(BaseModel):
    name: str

class Floor(BaseModel):
    level: int
    rooms: list[Room]

class Building(BaseModel):
    address: str
    floors: list[Floor]
```

Keep in mind:

- 2–3 levels of nesting is usually enough.
- Deeper structures make validation errors harder to trace.
- When nesting feels forced, consider flattening.

## Circular References

A model can reference itself — directly or indirectly. Use `model_rebuild()` to resolve it after the class is defined.

```python
from __future__ import annotations
from pydantic import BaseModel

class TreeNode(BaseModel):
    value: int
    children: list[TreeNode] = []

TreeNode.model_rebuild()
```

>[!IMPORTANT]
>**Always provide a default** (`[]` or `None`) for self-referential fields — without one, construction is impossible.

## Forward References

Reference a model before it's defined using string annotations or `from __future__ import annotations`.

```python
from __future__ import annotations
from pydantic import BaseModel

class Parent(BaseModel):
    name: str
    child: Child | None = None

class Child(BaseModel):
    name: str

Parent.model_rebuild()
```

Or inline with a string literal:

```python
class Parent(BaseModel):
    name: str
    child: "Child | None" = None
```

Call `model_rebuild()` after all referenced models are defined.

## Model Reusability

Define a model once, use it anywhere. Nested models are fully reusable across different parents.

```python
class Address(BaseModel):
    street: str
    city: str

class User(BaseModel):
    address: Address

class Company(BaseModel):
    headquarters: Address
    branch: Address
```

`Address` validation runs the same way in `User`, `Company`, or any other model — one definition, consistent behavior everywhere.
