# Multiple Interface Implementation in Python

Python doesn't have interfaces in the Java/C# sense, but the `abc` module lets you define **Abstract Base Classes (ABCs)** that serve the same purpose. A class can inherit from multiple ABCs simultaneously, giving it multiple "interfaces". This pattern lets you build flexible, composable systems where objects can fulfill several roles at once.

## Implementing Multiple Interfaces

To implement multiple interfaces, simply inherit from multiple ABCs. Python resolves method lookup using the **Method Resolution Order (MRO)** — left to right, depth first.

```python
from abc import ABC, abstractmethod

class Serializable(ABC):
    @abstractmethod
    def serialize(self) -> str:
        pass

class Validatable(ABC):
    @abstractmethod
    def validate(self) -> bool:
        pass

class Loggable(ABC):
    @abstractmethod
    def log_entry(self) -> str:
        pass


# Implementing all three interfaces
class UserRecord(Serializable, Validatable, Loggable):
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def serialize(self) -> str:
        return f'{{"name": "{self.name}", "age": {self.age}}}'

    def validate(self) -> bool:
        return bool(self.name) and self.age >= 0

    def log_entry(self) -> str:
        return f"[UserRecord] name={self.name}, age={self.age}"
```

>[!NOTE]
>If any abstract method is not implemented, Python raises `TypeError` at instantiation.

## Combining Behaviours Through Interfaces

Multiple interfaces let a single object act in different contexts without coupling those contexts together.

```python
def export_data(item: Serializable) -> str:
    return item.serialize()

def run_validation(item: Validatable) -> bool:
    return item.validate()

def write_log(item: Loggable) -> None:
    print(item.log_entry())

record = UserRecord("Alice", 30)

# Same object, used in three different contexts
export_data(record)      # → '{"name": "Alice", "age": 30}'
run_validation(record)   # → True
write_log(record)        # → [UserRecord] name=Alice, age=30
```

>[!NOTE]
>Each function only knows about one interface. The concrete class satisfies all three, but nothing is tightly coupled.

## Interface Composition Patterns

### Mixin Pattern

Mixins are small ABCs (or plain classes) that add one specific behaviour. They're meant to be composed, not used standalone.

```python
class TimestampMixin:
    def created_at(self) -> str:
        from datetime import datetime
        return datetime.utcnow().isoformat()

class AuditMixin:
    def audit_trail(self) -> list:
        return getattr(self, "_audit", [])

class Document(Serializable, TimestampMixin, AuditMixin):
    def __init__(self, content: str):
        self.content = content
        self._audit = []

    def serialize(self) -> str:
        return self.content
```

### Protocol-Based Composition (Python 3.8+)

`Protocol` from `typing` enables **structural subtyping** — no explicit inheritance needed.

```python
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None: ...

class Resizable(Protocol):
    def resize(self, factor: float) -> None: ...

# No ABC inheritance required — duck typing at type-check time
class Circle:
    def draw(self) -> None:
        print("Drawing circle")

    def resize(self, factor: float) -> None:
        self.radius *= factor
```

>[!TIP]
>Use `ABC` when you want **runtime enforcement**. Use `Protocol` when you want **static type checking without coupling**.

## Managing Method Name Conflicts

When two parent classes define the same method name, MRO determines which one is called. This can be a source of bugs if not handled carefully.

```python
class InterfaceA(ABC):
    def describe(self) -> str:
        return "From A"

class InterfaceB(ABC):
    def describe(self) -> str:
        return "From B"

class MyClass(InterfaceA, InterfaceB):
    pass

obj = MyClass()
obj.describe()  # → "From A"  (InterfaceA is first in MRO)
```

To call a specific parent explicitly:

```python
class MyClass(InterfaceA, InterfaceB):
    def describe(self) -> str:
        a_result = InterfaceA.describe(self)
        b_result = InterfaceB.describe(self)
        return f"{a_result} | {b_result}"
```

Check the MRO at any time with:

```python
print(MyClass.__mro__)
```

## Interface Hierarchies

ABCs can inherit from other ABCs, building interface hierarchies. This lets you group related capabilities and require subsets of behaviour.

```python
class Readable(ABC):
    @abstractmethod
    def read(self) -> str:
        pass

class Writable(ABC):
    @abstractmethod
    def write(self, data: str) -> None:
        pass

class ReadWritable(Readable, Writable):
    # Inherits both abstract methods — subclasses must implement both
    pass

class FileStream(ReadWritable):
    def __init__(self, path: str):
        self._path = path

    def read(self) -> str:
        with open(self._path) as f:
            return f.read()

    def write(self, data: str) -> None:
        with open(self._path, "w") as f:
            f.write(data)
```

>[!NOTE]
>A function typed to accept `Readable` will also accept `ReadWritable` — the hierarchy is transparent to callers.

## When to Use Multiple Interfaces

Use multiple interfaces when:

- An object genuinely plays **distinct roles** in different parts of your system (e.g., a record that is both serialisable and auditable).
- You want to **pass the same object to different subsystems** without those subsystems knowing about each other.
- You need **fine-grained type checking** — callers declare exactly which capability they need, nothing more.

Avoid multiple interfaces when:

- The "interfaces" are really just parts of one cohesive concept — use a single ABC instead.
- You're using inheritance for **code reuse alone** — prefer composition or plain helper classes.
- The class ends up implementing more than 4–5 interfaces — this is a signal the class is doing too much.

## Benefits of Multiple Interface Design

| Benefit | What it means in practice |
|---|---|
| **Loose coupling** | Subsystems depend only on the interface they use, not the full class |
| **Testability** | You can mock just the interface a function needs |
| **Flexibility** | Swap implementations freely as long as the interface contract is met |
| **Clarity** | Interfaces document intent — the class signature tells you what roles it plays |
| **Open/Closed** | Add new behaviour by implementing a new interface, not by modifying existing code |

## Common Pitfalls and Solutions

### 1. Diamond Problem

```python
class Base(ABC):
    def greet(self) -> str:
        return "Hello from Base"

class A(Base):
    def greet(self) -> str:
        return "Hello from A"

class B(Base):
    def greet(self) -> str:
        return "Hello from B"

class C(A, B):
    pass

C().greet()  # → "Hello from A"  — MRO resolves this predictably
```

**Solution:** Python's MRO (C3 linearisation) handles the diamond problem deterministically. Trust the MRO, but verify with `__mro__` when in doubt. Use `super()` consistently to ensure the full chain is called.

### 2. Forgetting to Implement an Abstract Method

```python
class Incomplete(Serializable):
    pass  # Missing serialize()

Incomplete()  # → TypeError: Can't instantiate abstract class
```

**Solution:** Implement all abstract methods, or mark the subclass abstract too if it's meant to be further subclassed.

### 3. Interface Bloat

Defining one large interface that forces implementors to provide methods they don't need.

```python
# Bad — forces all implementors to handle PDF, JSON, and XML
class Exporter(ABC):
    @abstractmethod
    def to_pdf(self): pass
    @abstractmethod
    def to_json(self): pass
    @abstractmethod
    def to_xml(self): pass

# Good — separate, composable interfaces
class PDFExporter(ABC):
    @abstractmethod
    def to_pdf(self): pass

class JSONExporter(ABC):
    @abstractmethod
    def to_json(self): pass
```

**Solution:** Follow the **Interface Segregation Principle** — keep interfaces small and focused. Combine them at the class level only when the class genuinely needs all capabilities.

### 4. Using `isinstance` Over Interface Typing

```python
# Fragile — checks for a concrete class
if isinstance(obj, UserRecord):
    obj.serialize()

# Robust — checks for the capability
if isinstance(obj, Serializable):
    obj.serialize()
```

**Solution:** Type-check against the ABC, not the concrete implementation. This keeps code open to new implementations.
