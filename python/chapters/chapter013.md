# Python3 Type Hints and Annotations
Type hints in Python3 allow developers to explicitly declare the expected data types of variables, function parameters, and return values. While Python remains dynamically typed, type hints improve code clarity, enable static analysis, and reduce runtime errors.

## Introduction to Type Hints
Introduced in **PEP 484** (Python 3.5+), type hints are optional annotations that indicate the intended type of a variable or expression.

```python
name: str = "Alice"
age: int = 30
pi: float = 3.14
active: bool = True
```

They do not enforce types at runtime — they are hints, not constraints.

## Annotating Function Parameters
Add type hints to function parameters using a colon after the parameter name.

```python
def greet(name: str, age: int) -> None:
    print(f"Hello, {name}. You are {age} years old.")
```

## Return Type Annotations
Specify a function's return type using `->` after the parameter list.

```python
def add(a: int, b: int) -> int:
    return a + b

def get_name() -> str:
    return "Alice"
```

## The None Type
Use `None` as the return type for functions that return nothing.

```python
def log_message(msg: str) -> None:
    print(msg)
```

Use `Optional` when a value can be a type *or* `None`.

```python
from typing import Optional

def find_user(user_id: int) -> Optional[str]:
    return "Alice" if user_id == 1 else None
```

## Benefits of Type Hints
- **Readability** — code is self-documenting.
- **IDE support** — better autocomplete and inline warnings.
- **Static analysis** — tools like `mypy` catch type errors before runtime.
- **Maintainability** — easier to refactor and onboard new developers.

## Type Hints for Collections
Use built-in generics (Python 3.9+) or `typing` module for collection types.

```python
# Python 3.9+ (preferred)
def get_scores(names: list[str]) -> dict[str, int]:
    return {name: 100 for name in names}

# Python 3.5–3.8 (using typing module)
from typing import List, Dict, Tuple, Set

def process(items: List[int]) -> Dict[str, int]:
    return {"total": sum(items)}

# Tuple with fixed types
def get_point() -> Tuple[int, int]:
    return (10, 20)

# Set
def unique_tags(tags: List[str]) -> Set[str]:
    return set(tags)
```
