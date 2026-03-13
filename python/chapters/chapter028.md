# Advanced Type Hints

Type hints make Python code easier to read, maintain, and debug. They don't affect runtime behavior — Python remains dynamically typed — but they unlock static analysis tools (like `mypy` or `pyright`) and serve as inline documentation. This guide covers the core tools for annotating complex, polymorphic, and generic code.

## The `typing` Module

The `typing` module (standard library) provides the building blocks for advanced annotations.

```python
from typing import List, Dict, Set, Tuple, Union, Optional, Any, TypeVar, Generic
```

>[!TIP]
>From Python 3.9+, built-in collections (`list`, `dict`, `set`, `tuple`) can be used directly as generic types, without importing from `typing`.

## Generic Types — `List`, `Dict`, `Set`, `Tuple`

Specify the types of elements a collection holds.

```python
from typing import List, Dict, Set, Tuple

# A list of strings
names: List[str] = ["Alice", "Bob"]

# A dict mapping strings to integers
scores: Dict[str, int] = {"Alice": 95, "Bob": 88}

# A set of floats
values: Set[float] = {1.1, 2.2, 3.3}

# A fixed-length tuple: (str, int, bool)
record: Tuple[str, int, bool] = ("Alice", 30, True)

# A variable-length tuple of ints
coords: Tuple[int, ...] = (10, 20, 30)
```

**Python 3.9+ shorthand:**

```python
names: list[str] = ["Alice", "Bob"]
scores: dict[str, int] = {"Alice": 95}
```

## Union Types

Use `Union` when a value can be one of several types.

```python
from typing import Union

def format_id(id: Union[int, str]) -> str:
    return str(id)
```

**Python 3.10+ shorthand** using the `|` operator:

```python
def format_id(id: int | str) -> str:
    return str(id)
```

## Optional Types

`Optional[X]` is shorthand for `Union[X, None]`. Use it when a value may be absent.

```python
from typing import Optional

def find_user(user_id: int) -> Optional[str]:
    users = {1: "Alice", 2: "Bob"}
    return users.get(user_id)  # Returns str or None
```

**Python 3.10+ shorthand:**

```python
def find_user(user_id: int) -> str | None:
    ...
```

## `Any` Type

`Any` opts out of type checking for a given value. Use it sparingly — it disables the benefits of static analysis.

```python
from typing import Any

def process(data: Any) -> Any:
    return data  # No type checking applied
```

>[!TIP]
>Useful when interfacing with untyped third-party code or during incremental migration to typed code.

## Type Aliases

Assign a name to a complex type to improve readability and reuse.

```python
from typing import List, Tuple

# Without alias — verbose
def get_matrix() -> List[List[float]]:
    ...

# With alias — clear and reusable
Matrix = List[List[float]]

def get_matrix() -> Matrix:
    ...

# A more descriptive alias
Coordinate = Tuple[float, float]
Point = Tuple[str, Coordinate]

location: Point = ("Paris", (48.8566, 2.3522))
```

**Python 3.10+ explicit alias syntax:**

```python
from typing import TypeAlias

Matrix: TypeAlias = list[list[float]]
```

## Generic Classes and Functions

Use `TypeVar` to write code that works with multiple types while remaining type-safe.

### Generic Functions

```python
from typing import TypeVar, List

T = TypeVar("T")

def first_item(items: List[T]) -> T:
    return items[0]

first_item([1, 2, 3])        # Returns int
first_item(["a", "b", "c"])  # Returns str
```

### Generic Classes

```python
from typing import TypeVar, Generic, List

T = TypeVar("T")

class Stack(Generic[T]):
    def __init__(self) -> None:
        self._items: List[T] = []

    def push(self, item: T) -> None:
        self._items.append(item)

    def pop(self) -> T:
        return self._items.pop()

int_stack: Stack[int] = Stack()
int_stack.push(10)
value: int = int_stack.pop()
```

### Bounded TypeVar

Restrict `T` to a specific base class or set of types.

```python
from typing import TypeVar

class Animal:
    def speak(self) -> str: ...

A = TypeVar("A", bound=Animal)

def make_speak(creature: A) -> str:
    return creature.speak()
```

## Type Hints for Polymorphic Code

Type hints integrate cleanly with OOP, ABCs, and polymorphism.

### Using Base Class / ABC as Return Type

```python
from abc import ABC, abstractmethod
from typing import List

class DataStream(ABC):
    @abstractmethod
    def read(self) -> List[str]: ...

class CSVStream(DataStream):
    def read(self) -> List[str]:
        return ["col1,col2", "val1,val2"]

class JSONStream(DataStream):
    def read(self) -> List[str]:
        return ['{"key": "value"}']

def process_stream(stream: DataStream) -> List[str]:
    return stream.read()
```

>[!NOTE]
>`process_stream` accepts any subclass of `DataStream` — a clean, type-safe pattern for subtype polymorphism.

### Protocol — Structural Subtyping

`Protocol` defines a structural interface (duck typing with type safety). A class satisfies a `Protocol` if it implements the required methods — no explicit inheritance needed.

```python
from typing import Protocol, List

class Readable(Protocol):
    def read(self) -> List[str]: ...

class FileSource:
    def read(self) -> List[str]:
        return ["line1", "line2"]

def load(source: Readable) -> List[str]:
    return source.read()

load(FileSource())  # Valid — FileSource matches the Protocol
```

### Combining Union with Polymorphism

```python
from typing import Union

class IntStream:
    def read(self) -> list[int]: ...

class FloatStream:
    def read(self) -> list[float]: ...

StreamType = Union[IntStream, FloatStream]

def dispatch(stream: StreamType) -> None:
    data = stream.read()
    print(data)
```

## Quick Reference

| Construct | Purpose | Example |
|---|---|---|
| `List[T]` | Typed list | `List[str]` |
| `Dict[K, V]` | Typed dict | `Dict[str, int]` |
| `Tuple[T, ...]` | Typed tuple | `Tuple[int, str]` |
| `Union[X, Y]` | Either type | `Union[int, str]` |
| `Optional[X]` | Value or None | `Optional[str]` |
| `Any` | No type check | `Any` |
| `TypeVar` | Generic placeholder | `T = TypeVar("T")` |
| `Generic[T]` | Generic class base | `class Box(Generic[T])` |
| `Protocol` | Structural interface | `class Readable(Protocol)` |
| `TypeAlias` | Named type alias | `Matrix: TypeAlias = list[list[float]]` |
```
