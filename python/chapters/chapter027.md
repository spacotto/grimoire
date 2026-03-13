# Protocols and Duck Typing

Python's type system supports two complementary approaches to polymorphism: **nominal subtyping** (class hierarchies via inheritance) and **structural subtyping** (shape-based compatibility). Protocols formalise the second approach, giving duck typing a static, checkable form without requiring shared base classes.

## What Are Protocols?

A **Protocol** defines a set of methods and attributes that a type must expose — without requiring that type to explicitly inherit from anything.

>[!TIP]
>Think of it as a contract based on *shape*, not *lineage*:
>"I don't care what class you are. I care whether you have a `.read()` method."

Protocols were introduced in **PEP 544** and are available via the `typing` module since Python 3.8.

```python
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None: ...
```

>[!NOTE]
>Any class that implements `draw()` is compatible with `Drawable` — no inheritance needed.

## Structural Subtyping

Python traditionally uses **nominal subtyping**: a class is a subtype of another only if it explicitly inherits from it. **Structural subtyping** is different — a class is compatible with a Protocol if it has the required structure (methods, attributes, signatures), regardless of its class hierarchy.

```python
class Circle:
    def draw(self) -> None:
        print("Drawing a circle")

class Square:
    def draw(self) -> None:
        print("Drawing a square")

def render(shape: Drawable) -> None:
    shape.draw()

render(Circle())  # ✓ — structurally compatible
render(Square())  # ✓ — structurally compatible
```

>[!NOTE]
>Neither `Circle` nor `Square` inherits from `Drawable`, but both satisfy its structure.

## The `typing.Protocol` Class

Import `Protocol` from `typing` (Python 3.8+) or `typing_extensions` for older versions.

```python
from typing import Protocol

class Serializable(Protocol):
    def to_json(self) -> str: ...
    def from_json(self, data: str) -> None: ...
```

Key rules:
- Methods are declared with `...` as the body (or `pass`).
- Protocol classes should not hold implementation logic (with exceptions — see below).
- A class implicitly satisfies a Protocol if it implements all required members with matching signatures.

## Defining Protocol Interfaces

Protocols can define methods, properties, and class variables.

```python
from typing import Protocol

class DataStore(Protocol):
    name: str                           # attribute

    def save(self, record: dict) -> None: ...
    def load(self, key: str) -> dict: ...

    @property
    def is_connected(self) -> bool: ...
```

Protocols can also inherit from other Protocols to compose interfaces:

```python
class ReadableStore(Protocol):
    def load(self, key: str) -> dict: ...

class WritableStore(Protocol):
    def save(self, record: dict) -> None: ...

class ReadWriteStore(ReadableStore, WritableStore, Protocol):
    ...
```

>[!NOTE]
>When composing Protocols via inheritance, always include `Protocol` explicitly in the base list to keep the composed class a Protocol itself.

## Duck Typing ("If it walks like a duck...")

Duck typing is Python's original philosophy: *"If it walks like a duck and quacks like a duck, it is a duck."*

Python doesn't check types at runtime by default — it checks whether an object *supports the required operations* when they're called.

```python
class Dog:
    def speak(self) -> str:
        return "Woof"

class Person:
    def speak(self) -> str:
        return "Hello"

def make_noise(entity) -> None:
    print(entity.speak())   # Works for any object with .speak()

make_noise(Dog())     # Woof
make_noise(Person())  # Hello
```

>[!NOTE]
>Duck typing is dynamic and flexible, but offers no static guarantees. Protocols formalise this pattern with type annotations — you get duck typing's flexibility *with* static analysis support.

## Protocol vs. ABC

Both Protocols and Abstract Base Classes (ABCs) define interfaces. They serve different purposes.

| Feature | Protocol | ABC |
|---|---|---|
| Requires explicit inheritance | No | Yes |
| Subtyping style | Structural | Nominal |
| Runtime enforcement | Only with `@runtime_checkable` | Yes (via `abstractmethod`) |
| Works with third-party classes | Yes | Only if they inherit |
| Static type checking | Yes | Yes |

**Use an ABC when:**
- You want to enforce implementation via inheritance.
- You share implementation logic in a base class.
- You need runtime `isinstance()` checks guaranteed.

**Use a Protocol when:**
- You want to describe an interface without coupling classes.
- You're working with third-party or built-in types you can't modify.
- You prefer structural flexibility.

```python
from abc import ABC, abstractmethod

# ABC approach — requires inheritance
class Shape(ABC):
    @abstractmethod
    def area(self) -> float: ...

class Rectangle(Shape):   # must explicitly inherit
    def area(self) -> float:
        return self.width * self.height

# Protocol approach — no inheritance needed
class HasArea(Protocol):
    def area(self) -> float: ...

class Triangle:           # no inheritance
    def area(self) -> float:
        return 0.5 * self.base * self.height
```

---

## Runtime Checkable Protocols

By default, Protocols are *static-only* — you cannot use `isinstance()` with them at runtime.

To enable runtime checks, decorate the Protocol with `@runtime_checkable`:

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Flyable(Protocol):
    def fly(self) -> None: ...

class Bird:
    def fly(self) -> None:
        print("Flapping wings")

b = Bird()
print(isinstance(b, Flyable))   # True
```

>[!WARNING]
>**Limitation:** Runtime checks only verify that the *methods exist*, not that their signatures match. Use static type checkers (e.g., mypy, pyright) for full signature validation.

```python
class Fake:
    fly = "not a method"   # exists, but wrong type

print(isinstance(Fake(), Flyable))  # True — runtime check is shallow
```

## When to Use Protocols

Use Protocols when you want to write flexible, decoupled interfaces that work with any compatible type.

**Good candidates:**
- Functions or classes that should work with multiple unrelated types.
- Library code where consumers can't (or shouldn't) inherit from your base classes.
- Replacing `Any` type hints with something more precise.
- Describing the interface of built-in or third-party objects.

```python
from typing import Protocol

class Closeable(Protocol):
    def close(self) -> None: ...

def shutdown(resource: Closeable) -> None:
    resource.close()

# Works with file objects, sockets, DB connections — anything with .close()
```

**Avoid Protocols when:**
- You need shared implementation logic (use ABCs or regular base classes instead).
- You need guaranteed enforcement at object construction time.
- Your team is unfamiliar with structural subtyping and readability is a priority.

>[!NOTE]
>**Summary:** Protocols bring static typing support to Python's duck typing tradition. They let you describe *what an object can do* — not *what it inherits from* — making your code more composable, testable, and open to extension.
