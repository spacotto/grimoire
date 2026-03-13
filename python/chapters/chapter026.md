# Abstract Base Classes (ABC) in Python

Abstract Base Classes (ABC) define a shared interface for a group of related classes. They specify *what* methods a subclass must implement, without dictating *how*. ABCs are Python's primary tool for enforcing interface contracts in object-oriented design.

## What are Abstract Base Classes (ABC)?

An **Abstract Base Class** is a class that cannot be instantiated directly. It serves as a blueprint: it declares methods that all subclasses *must* implement.

ABCs allow you to:
- Define a common interface across multiple classes
- Enforce method implementation at class definition time
- Communicate intent clearly to other developers

```python
# Without ABC: no enforcement, silent bugs
class Animal:
    def speak(self):
        pass  # Nothing forces subclasses to implement this

class Dog(Animal):
    pass  # Forgot speak() — no error raised

d = Dog()
d.speak()  # Runs silently, returns None
```

## The `abc` Module

Python's `abc` module provides the tools to define abstract base classes.

```python
from abc import ABC, abstractmethod
```

Two key imports:
- `ABC` — the base class to inherit from
- `abstractmethod` — the decorator that marks a method as abstract

## The `ABC` Base Class

To make a class abstract, inherit from `ABC`.

```python
from abc import ABC

class Shape(ABC):
    pass
```

`ABC` is a convenience class. Under the hood, it uses `ABCMeta` as its metaclass — the actual mechanism that enforces abstract method contracts.

```python
# Equivalent, more explicit form:
from abc import ABCMeta

class Shape(metaclass=ABCMeta):
    pass
```

>[!TIP]
>Both approaches are valid. `ABC` is preferred for its simplicity.

## The `@abstractmethod` Decorator

`@abstractmethod` marks a method as abstract: it must be overridden in any concrete subclass.

```python
from abc import ABC, abstractmethod

class Shape(ABC):

    @abstractmethod
    def area(self) -> float:
        pass
```

Rules:
- The abstract method body is typically `pass` or a docstring
- Any class with at least one unimplemented abstract method is itself abstract
- Subclasses that don't implement all abstract methods remain abstract

## Defining Abstract Methods

Abstract methods define the *signature* of what subclasses must provide.

```python
from abc import ABC, abstractmethod

class DataStream(ABC):

    @abstractmethod
    def read(self) -> str:
        """Read data from the stream."""
        pass

    @abstractmethod
    def write(self, data: str) -> None:
        """Write data to the stream."""
        pass
```

The body of an abstract method is ignored at runtime. It exists only to:
- Provide a docstring/documentation
- Offer a callable default (via `super()`) when needed

## Implementing Abstract Methods in Subclasses

A concrete subclass must implement **every** abstract method defined in the ABC.

```python
class FileStream(DataStream):

    def read(self) -> str:
        return "reading from file"

    def write(self, data: str) -> None:
        print(f"writing to file: {data}")
```

**Rules:**
- Method signatures should match (same name, compatible parameters)
- The implementation replaces the abstract placeholder entirely
- Once all abstract methods are implemented, the class is **concrete** and can be instantiated

## Cannot Instantiate Abstract Classes

Attempting to instantiate an abstract class raises a `TypeError`.

```python
stream = DataStream()
# TypeError: Can't instantiate abstract class DataStream
# with abstract methods read, write
```

This is enforced automatically by `ABCMeta`. The error message lists which methods are missing — useful for debugging.

```python
# Works only after all abstract methods are implemented:
stream = FileStream()  # OK
```

## Abstract Properties

Use `@property` combined with `@abstractmethod` to enforce abstract properties.

```python
from abc import ABC, abstractmethod

class Config(ABC):

    @property
    @abstractmethod
    def timeout(self) -> int:
        pass
```

Subclasses must implement `timeout` as a property:

```python
class AppConfig(Config):

    @property
    def timeout(self) -> int:
        return 30
```

>[!IMPORTANT]
>**Order matters:** `@property` must come *before* `@abstractmethod`.

## Enforcing Interface Contracts

The primary purpose of ABCs is to enforce a contract: any class claiming to be a `Shape`, `Stream`, or `Processor` must implement the required interface.

```python
from abc import ABC, abstractmethod

class Processor(ABC):

    @abstractmethod
    def process(self, data: list) -> list:
        pass

    @abstractmethod
    def validate(self, data: list) -> bool:
        pass


class CSVProcessor(Processor):
    # Must implement both process() and validate()

    def process(self, data: list) -> list:
        return [row.strip() for row in data]

    def validate(self, data: list) -> bool:
        return all(isinstance(row, str) for row in data)
```

If `CSVProcessor` omits either method, instantiation fails immediately — not silently at call time.

## When to Use Abstract Base Classes

Use ABCs when:

| Situation | Why ABCs help |
|---|---|
| Multiple classes share a common interface | Guarantees consistent API across all implementations |
| You want to prevent direct instantiation of a base class | ABCs enforce this automatically |
| You're building a plugin or extension system | Third-party code must conform to your interface |
| You want to communicate design intent clearly | ABCs are self-documenting contracts |

Avoid ABCs when:
- A simple base class with concrete methods is sufficient
- You're working with small, tightly controlled codebases where duck typing is adequate
- The overhead of formal contracts adds complexity without benefit

## Abstract Methods vs. Concrete Methods

ABCs can contain both abstract and concrete methods.

| | Abstract Method | Concrete Method |
|---|---|---|
| Defined with | `@abstractmethod` | No decorator |
| Body | `pass` or docstring | Full implementation |
| Must be overridden? | **Yes** | No (but can be) |
| Purpose | Enforce interface | Provide shared logic |

```python
class Stream(ABC):

    @abstractmethod
    def read(self) -> str:           # Must be overridden
        pass

    def log(self, message: str):     # Shared, ready to use
        print(f"[LOG] {message}")
```

>[!NOTE]
>Subclasses inherit concrete methods and can call them directly — no reimplementation needed.

## Partial Implementation in Abstract Classes

An abstract class can implement *some* abstract methods from its own parent, leaving others for further subclasses.

```python
class BaseStream(ABC):

    @abstractmethod
    def read(self) -> str:
        pass

    @abstractmethod
    def write(self, data: str) -> None:
        pass


class ReadOnlyStream(BaseStream):
    # Implements read(), leaves write() abstract

    def read(self) -> str:
        return "data"

    # write() still abstract — ReadOnlyStream cannot be instantiated


class ConcreteReadOnlyStream(ReadOnlyStream):
    # Must implement write() to become concrete

    def write(self, data: str) -> None:
        raise NotImplementedError("This stream is read-only.")
```

This pattern is useful for building class hierarchies where:
- Intermediate classes provide partial shared behavior
- Leaf classes complete the full implementation
