# Interface Design Principles in Python

Interfaces define contracts — agreements about what an object *can do*, not *how it does it*. In Python, interfaces are implemented via Abstract Base Classes (ABCs) or structural typing (Protocols). Well-designed interfaces make systems easier to extend, test, and maintain. This document covers core principles for designing clean, minimal, and composable interfaces.

## What are Interfaces?

An **interface** specifies a set of methods a class must implement, without providing their logic. It answers: *"What can this object do?"* In Python, the closest built-in mechanism is the `ABC` (Abstract Base Class) from the `abc` module, or `Protocol` from `typing` for structural subtyping.

```python
from abc import ABC, abstractmethod

class Serializable(ABC):
    @abstractmethod
    def serialize(self) -> str:
        """Convert object to a string representation."""
        ...

    @abstractmethod
    def deserialize(self, data: str) -> None:
        """Populate object from a string representation."""
        ...
```

>[!IMPORTANT]
>Any class that inherits from `Serializable` **must** implement both methods, or it cannot be instantiated.

## Interfaces vs. Abstract Classes

| Feature | Interface | Abstract Class |
|---|---|---|
| Contains logic? | No | Yes (partially) |
| Holds state? | No | Yes |
| Purpose | Define a contract | Provide shared base behaviour |
| Python tool | `Protocol`, `ABC` (no logic) | `ABC` (with concrete methods) |

**Use an interface** when you only care that an object supports certain operations.  
**Use an abstract class** when subclasses should share common logic or state.

```python
from abc import ABC, abstractmethod

# Abstract class — shared logic included
class BaseExporter(ABC):
    def export(self, data: list) -> None:
        validated = self._validate(data)       # shared step
        self._write(validated)                 # delegated to subclass

    def _validate(self, data: list) -> list:
        return [item for item in data if item]

    @abstractmethod
    def _write(self, data: list) -> None: ...  # subclass defines this


# Interface-style ABC — no logic, just contract
class Writable(ABC):
    @abstractmethod
    def write(self, content: str) -> None: ...
```

## Interface Segregation Principle

> *"Clients should not be forced to depend on methods they do not use."*  
> — Robert C. Martin (ISP, part of SOLID)

**Split large interfaces into smaller, focused ones.** A class should only implement what it actually needs.

```python
# ❌ Fat interface — forces all implementors to define methods they may not need
class DataHandler(ABC):
    @abstractmethod
    def read(self) -> str: ...
    @abstractmethod
    def write(self, data: str) -> None: ...
    @abstractmethod
    def delete(self) -> None: ...
    @abstractmethod
    def archive(self) -> None: ...


# ✅ Segregated interfaces — each class picks what applies
class Readable(ABC):
    @abstractmethod
    def read(self) -> str: ...

class Writable(ABC):
    @abstractmethod
    def write(self, data: str) -> None: ...

class Deletable(ABC):
    @abstractmethod
    def delete(self) -> None: ...


class ReadOnlyStream(Readable):
    def read(self) -> str:
        return "data"

class ReadWriteStream(Readable, Writable):
    def read(self) -> str:
        return "data"

    def write(self, data: str) -> None:
        print(f"Writing: {data}")
```

## Designing Minimal Interfaces

A good interface does **one thing**. Every method should be essential — if you can remove it without breaking the contract, it shouldn't be there.

**Questions to ask when designing an interface:**
- Does every method belong to the same concept?
- Would any implementor reasonably need to leave a method empty?
- Can this be split into two smaller, more focused interfaces?

```python
# ❌ Overloaded — mixes concerns
class StreamProcessor(ABC):
    @abstractmethod
    def read(self) -> str: ...
    @abstractmethod
    def process(self, data: str) -> str: ...
    @abstractmethod
    def log(self, message: str) -> None: ...  # logging ≠ processing


# ✅ Minimal — each interface has a single responsibility
class Readable(ABC):
    @abstractmethod
    def read(self) -> str: ...

class Processable(ABC):
    @abstractmethod
    def process(self, data: str) -> str: ...

class Loggable(ABC):
    @abstractmethod
    def log(self, message: str) -> None: ...
```

## Interface Composition

Build complex behaviour by **combining small interfaces**, rather than creating large monolithic ones. This keeps each piece focused and reusable.

```python
from abc import ABC, abstractmethod

class Readable(ABC):
    @abstractmethod
    def read(self) -> str: ...

class Writable(ABC):
    @abstractmethod
    def write(self, data: str) -> None: ...

class Closable(ABC):
    @abstractmethod
    def close(self) -> None: ...


# Composed interface via multiple inheritance
class ReadWriteStream(Readable, Writable, Closable):
    def read(self) -> str:
        return "stream data"

    def write(self, data: str) -> None:
        print(f"Writing: {data}")

    def close(self) -> None:
        print("Stream closed.")


def process_stream(stream: Readable) -> str:
    # Only requires Readable — doesn't care about the rest
    return stream.read().upper()
```

>[!TIP]
>Functions should accept **the narrowest interface** they actually need. `process_stream` only needs `Readable`, so that's all it asks for.

## Cohesive Interface Design

**Cohesion** means all methods in an interface belong together — they serve one unified purpose.

A cohesive interface groups methods that:
- Operate on the same data
- Belong to the same workflow stage
- Would always be implemented together

```python
# ❌ Low cohesion — unrelated methods mixed in one interface
class MixedInterface(ABC):
    @abstractmethod
    def parse(self, raw: str) -> dict: ...       # parsing
    @abstractmethod
    def send_email(self, to: str) -> None: ...   # notification
    @abstractmethod
    def save_to_db(self, record: dict) -> None:  # persistence
        ...


# ✅ High cohesion — each interface has a clear, single concern
class Parser(ABC):
    @abstractmethod
    def parse(self, raw: str) -> dict: ...

class Notifier(ABC):
    @abstractmethod
    def send_email(self, to: str) -> None: ...

class Repository(ABC):
    @abstractmethod
    def save(self, record: dict) -> None: ...
```

## Contract-Based Programming

An interface is a **contract**: implementors promise to fulfil specific behaviour, and callers rely on that promise — without knowing the underlying implementation.

This enables:
- **Swappable implementations** (e.g. swap a real DB for a mock in tests)
- **Decoupled components** (modules depend on abstractions, not concretions)

```python
from abc import ABC, abstractmethod

class DataStore(ABC):
    @abstractmethod
    def save(self, key: str, value: str) -> None: ...

    @abstractmethod
    def load(self, key: str) -> str: ...


class InMemoryStore(DataStore):
    def __init__(self) -> None:
        self._store: dict[str, str] = {}

    def save(self, key: str, value: str) -> None:
        self._store[key] = value

    def load(self, key: str) -> str:
        return self._store.get(key, "")


class FileStore(DataStore):
    def save(self, key: str, value: str) -> None:
        with open(f"{key}.txt", "w") as f:
            f.write(value)

    def load(self, key: str) -> str:
        with open(f"{key}.txt") as f:
            return f.read()


# Caller depends only on the contract — not the implementation
def persist(store: DataStore, key: str, value: str) -> None:
    store.save(key, value)
    print(f"Saved '{key}' → '{value}'")
```

>[!NOTE]
>Swap `InMemoryStore` for `FileStore` — `persist` doesn't change.

## Interface Documentation

Document **what the method must do**, not how. Callers rely on the docstring; implementors follow it as a spec.

```python
from abc import ABC, abstractmethod

class Tokenizer(ABC):
    @abstractmethod
    def tokenize(self, text: str) -> list[str]:
        """
        Split text into a list of tokens.

        Args:
            text: The raw input string to tokenize.

        Returns:
            A list of string tokens. Must not return None.
            Returns an empty list if text is empty or whitespace.

        Raises:
            ValueError: If text is not a string.
        """
        ...

    @abstractmethod
    def detokenize(self, tokens: list[str]) -> str:
        """
        Reconstruct a string from a list of tokens.

        Args:
            tokens: A list of string tokens (may be empty).

        Returns:
            The reconstructed string. Must be the inverse of tokenize
            for well-formed input.
        """
        ...
```
**Good interface docs include:**
- What the method **must** return (including edge cases like empty input)
- What exceptions it **may** raise
- Any invariants or constraints the implementation must respect
- What it must **not** do (side effects, mutation of inputs, etc.)
