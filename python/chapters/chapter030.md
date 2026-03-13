# Building Polymorphic Systems

Polymorphism lets different objects respond to the same interface in their own way. In Python, this means you can write code that works with a *type of thing* rather than a specific thing — and that code keeps working as you add new types later. This chapter covers how to design, implement, and maintain polymorphic systems in Python 3: from foundational principles to practical patterns for batch processing and error handling.

## Designing for Extensibility

An extensible system grows without requiring rewrites. The goal is to add new behaviour by *adding* code, not by changing existing code.

Start by identifying what varies. If you have logic that branches on type — `if isinstance(obj, A)` … `elif isinstance(obj, B)` — that's a signal to use polymorphism instead.

```python
# Fragile: adding a new type means editing this function
def process(stream):
    if isinstance(stream, CSVStream):
        return stream.parse_csv()
    elif isinstance(stream, JSONStream):
        return stream.parse_json()

# Extensible: each type knows how to process itself
def process(stream):
    return stream.parse()  # works for any stream type
```

>[!TIP]
>Design around *behaviours*, not *types*.

## Open/Closed Principle

>[!IMPORTANT]
>A class should be open for extension, but closed for modification.

Once a class works correctly, you shouldn't need to change it to support new use cases. New behaviour comes from new subclasses or implementations, not from editing existing code.

```python
from abc import ABC, abstractmethod

class DataStream(ABC):
    @abstractmethod
    def read(self) -> list[dict]:
        pass

    @abstractmethod
    def get_format(self) -> str:
        pass


class CSVStream(DataStream):
    def __init__(self, filepath: str):
        self.filepath = filepath

    def read(self) -> list[dict]:
        import csv
        with open(self.filepath) as f:
            return list(csv.DictReader(f))

    def get_format(self) -> str:
        return "csv"


class JSONStream(DataStream):
    def __init__(self, filepath: str):
        self.filepath = filepath

    def read(self) -> list[dict]:
        import json
        with open(self.filepath) as f:
            return json.load(f)

    def get_format(self) -> str:
        return "json"
```

>[!WARNING]
>`DataStream` is closed for modification. To support XML, you add `XMLStream` — you don't touch `CSVStream` or `JSONStream`.

## Liskov Substitution Principle

>[!IMPORTANT]
>A subclass should be usable wherever its parent is expected.

If code works with a `DataStream`, it must work with *any* `DataStream` subclass — without the caller needing to know which subclass it has.

This means subclasses must:
- Honour the same method signatures
- Not raise unexpected exceptions
- Not weaken preconditions or strengthen postconditions

```python
def summarize(stream: DataStream) -> None:
    # Works with CSVStream, JSONStream, or any future subclass
    records = stream.read()
    print(f"Format: {stream.get_format()} | Records: {len(records)}")
```

**What breaks LSP:**

```python
class BrokenStream(DataStream):
    def read(self) -> list[dict]:
        raise NotImplementedError("Not supported")  # violates contract

    def get_format(self) -> str:
        return None  # wrong return type — callers expect a string
```

>[!NOTE]
>A subclass that can't fulfil its parent's contract shouldn't inherit from it.

## Interface-Based Programming

Program against the *interface*, not the *implementation*. In Python, interfaces are defined using abstract base classes (`ABC`) or structural typing (duck typing / `Protocol`).

**Using `ABC` for explicit contracts:**

```python
from abc import ABC, abstractmethod

class StreamProcessor(ABC):
    @abstractmethod
    def process(self, data: list[dict]) -> list[dict]:
        pass

    @abstractmethod
    def get_stats(self) -> dict:
        pass
```

**Using `Protocol` for structural typing (no inheritance required):**

```python
from typing import Protocol

class Readable(Protocol):
    def read(self) -> list[dict]: ...
    def get_format(self) -> str: ...


def load(source: Readable) -> list[dict]:
    print(f"Loading from {source.get_format()}")
    return source.read()
```

>[!NOTE]
>Any class with a matching `read` and `get_format` method satisfies `Readable` — no explicit inheritance needed. This is useful when you can't modify third-party classes.

>[!TIP]
>Use `ABC` when you want enforced inheritance and shared base logic. Use `Protocol` when you want flexible, duck-typed interfaces.

## Processing Mixed Types Polymorphically

One of the key payoffs of polymorphism: you can hold a mixed collection of objects and process them uniformly.

```python
streams: list[DataStream] = [
    CSVStream("data/users.csv"),
    JSONStream("data/orders.json"),
    CSVStream("data/products.csv"),
]

all_records = []
for stream in streams:
    records = stream.read()       # same call, different behaviour
    all_records.extend(records)

print(f"Total records loaded: {len(all_records)}")
```

>[!NOTE]
>The loop doesn't care what kind of stream each item is. Each object handles its own `read()` logic — the caller stays clean.

>[!TIP]
>This pattern scales: add a new stream type, and the loop just works.

## Batch Processing with Polymorphism

Batch operations often combine a pipeline of processors applied to a collection. Polymorphism makes the pipeline composable and easy to extend.

```python
class FilterProcessor(StreamProcessor):
    def __init__(self, key: str, value: str):
        self.key = key
        self.value = value
        self._processed = 0
        self._passed = 0

    def process(self, data: list[dict]) -> list[dict]:
        self._processed = len(data)
        result = [row for row in data if row.get(self.key) == self.value]
        self._passed = len(result)
        return result

    def get_stats(self) -> dict:
        return {
            "processed": self._processed,
            "passed": self._passed,
            "filtered_out": self._processed - self._passed,
        }


class Pipeline:
    def __init__(self, processors: list[StreamProcessor]):
        self.processors = processors

    def run(self, data: list[dict]) -> list[dict]:
        for processor in self.processors:
            data = processor.process(data)
        return data

    def report(self) -> None:
        for i, processor in enumerate(self.processors):
            stats = processor.get_stats()
            print(f"Step {i + 1} ({type(processor).__name__}): {stats}")
```

>[!TIP]
>Each processor in the pipeline is interchangeable. To add a new transformation step, implement `StreamProcessor` and insert it — no changes to `Pipeline`.

## Error Handling in Polymorphic Systems

Each subclass may fail in different ways, but callers should receive consistent error types. Define a hierarchy of domain exceptions and raise them at the boundary.

```python
class StreamError(Exception):
    """Base exception for all stream errors."""
    pass

class ReadError(StreamError):
    """Raised when reading from a stream fails."""
    pass

class ParseError(StreamError):
    """Raised when data cannot be parsed."""
    pass


class CSVStream(DataStream):
    def read(self) -> list[dict]:
        import csv
        try:
            with open(self.filepath) as f:
                return list(csv.DictReader(f))
        except FileNotFoundError as e:
            raise ReadError(f"CSV file not found: {self.filepath}") from e
        except csv.Error as e:
            raise ParseError(f"CSV parse error in {self.filepath}: {e}") from e

    def get_format(self) -> str:
        return "csv"
```

Callers catch `StreamError` (or its subtypes) regardless of which stream type they're working with:

```python
def safe_load(stream: DataStream) -> list[dict]:
    try:
        return stream.read()
    except ReadError as e:
        print(f"Could not read: {e}")
        return []
    except ParseError as e:
        print(f"Could not parse: {e}")
        return []
```

>[!NOTE]
>This keeps error handling uniform across the system. Subclasses handle implementation-specific failures; callers handle domain-level failures.

## Performance Considerations

Polymorphism adds flexibility, but it comes with trade-offs worth knowing.

### Method dispatch overhead

**Method dispatch overhead** is minimal in most applications. Don't optimise prematurely — profile first.

### Memory

Each instance carries a reference to its class. For very large collections of small objects, `__slots__` reduces per-instance overhead:

```python
class CSVStream(DataStream):
    __slots__ = ("filepath",)

    def __init__(self, filepath: str):
        self.filepath = filepath
```

### Lazy loading

If initialising a stream is expensive, defer it:

```python
class LazyCSVStream(DataStream):
    def __init__(self, filepath: str):
        self.filepath = filepath
        self._cache: list[dict] | None = None

    def read(self) -> list[dict]:
        if self._cache is None:
            import csv
            with open(self.filepath) as f:
                self._cache = list(csv.DictReader(f))
        return self._cache

    def get_format(self) -> str:
        return "csv"
```

### Batch size

When processing large datasets polymorphically, prefer generators over loading everything into memory at once:

```python
from typing import Iterator

class StreamProcessor(ABC):
    @abstractmethod
    def process_iter(self, data: Iterator[dict]) -> Iterator[dict]:
        pass
```

>[!IMPORTANT]
>Keep the interface clean and the implementation efficient. Polymorphism and performance are not in conflict — they just require deliberate design.
