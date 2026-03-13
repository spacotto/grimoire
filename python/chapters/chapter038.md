# Advanced Abstraction Techniques in Python 3

Abstraction is the practice of hiding complexity behind a simpler interface. In Python, this means exposing only what a caller needs to know — and nothing more. Good abstractions make code easier to read, test, and change. Poor ones make it harder. This document covers the core concepts and practical techniques for working with abstraction at an advanced level.

## Layered Abstractions

Real systems are built in layers. Each layer abstracts over the one below it and exposes a cleaner interface to the one above.

```python
# Low-level: raw file I/O
def read_bytes(filepath: str) -> bytes:
    with open(filepath, "rb") as f:
        return f.read()

# Mid-level: decoded text
def read_text(filepath: str, encoding: str = "utf-8") -> str:
    return read_bytes(filepath).decode(encoding)

# High-level: structured config
def load_config(filepath: str) -> dict:
    import json
    return json.loads(read_text(filepath))
```

>[!NOTE]
>Each layer has one job. The high-level caller doesn't need to know about bytes or encoding — that complexity lives below.

>[!TIP]
>**Rule of thumb:** if a layer is doing two things, it probably belongs at two different levels.

## Abstraction Levels

Python gives you several tools to express different levels of abstraction:

| Level | Tool | Use case |
|---|---|---|
| Function | `def` | Encapsulate a single operation |
| Class | `class` | Encapsulate state + behavior |
| Interface | `ABC` / `Protocol` | Define a contract without implementation |
| Module | `.py` file | Group related abstractions |
| Package | directory + `__init__.py` | Group related modules |

```python
from abc import ABC, abstractmethod

# Interface level: defines the contract
class Serializer(ABC):
    @abstractmethod
    def serialize(self, data: dict) -> str: ...

    @abstractmethod
    def deserialize(self, raw: str) -> dict: ...

# Implementation level: fulfills the contract
class JSONSerializer(Serializer):
    import json

    def serialize(self, data: dict) -> str:
        return self.json.dumps(data)

    def deserialize(self, raw: str) -> dict:
        return self.json.loads(raw)
```

>[!NOTE]
>The `Serializer` ABC defines *what* must be done. `JSONSerializer` defines *how*.

## Leaky Abstractions

A leaky abstraction is one that forces callers to understand the details it was supposed to hide. This is a common failure mode.

```python
# Leaky: the caller must know about internal pagination
class UserRepository:
    def get_users(self, page: int, page_size: int) -> list:
        # exposes database pagination directly
        return self.db.query(f"SELECT * FROM users LIMIT {page_size} OFFSET {page * page_size}")

# Better: hide the pagination detail
class UserRepository:
    def get_all_users(self) -> list:
        results = []
        page = 0
        while chunk := self._fetch_page(page):
            results.extend(chunk)
            page += 1
        return results

    def _fetch_page(self, page: int) -> list:
        page_size = 100
        return self.db.query(f"SELECT * FROM users LIMIT {page_size} OFFSET {page * page_size}")
```

Signs of a leaky abstraction:
- Callers need to pass internal implementation details as arguments
- Error messages reference internals the caller never touched
- Callers need to call methods in a specific order to avoid bugs

## Abstraction Trade-offs

Every abstraction has a cost. The question is whether it pays for itself.

```python
# Direct: simple, fast to write, easy to read in isolation
total = sum(item["price"] * item["qty"] for item in cart)

# Abstracted: more code, but reusable and testable
def calculate_total(cart: list[dict]) -> float:
    return sum(item["price"] * item["qty"] for item in cart)
```

| Benefit | Cost |
|---|---|
| Reusability | Indirection (harder to trace) |
| Testability | More code to maintain |
| Replaceability | Learning curve for new contributors |
| Encapsulation | Premature complexity if used too early |

>[!TIP]
>A good abstraction earns back its cost through reuse, stability, or clarity. A bad one just adds indirection.

## When to Abstract

A practical heuristic: **abstract when you feel the pain, not before.**

```python
# First time: just write it inline
report = "\n".join(f"{k}: {v}" for k, v in metrics.items())

# Second time: same pattern, same place — still fine inline

# Third time: now it's worth abstracting
def format_metrics(metrics: dict) -> str:
    return "\n".join(f"{k}: {v}" for k, v in metrics.items())
```

Abstract when:
- The same logic appears in 3+ places
- Implementation details are changing and you want to protect callers
- You need to swap one implementation for another (e.g., mock in tests)
- A piece of code has grown large enough to obscure the intent of its caller

Do not abstract when:
- You're writing something for the first time
- You're speculating about future reuse
- The abstraction would be harder to read than the raw code

## Over-Abstraction Pitfalls

Over-abstraction is when the structure of the code becomes the main thing you're working with, instead of the problem itself.

```python
# Over-abstracted: 4 classes to add two numbers
class Operand:
    def __init__(self, value): self.value = value

class AdditionStrategy:
    def execute(self, a: Operand, b: Operand) -> Operand:
        return Operand(a.value + b.value)

class Calculator:
    def __init__(self, strategy: AdditionStrategy):
        self.strategy = strategy

    def compute(self, a, b):
        return self.strategy.execute(Operand(a), Operand(b))

result = Calculator(AdditionStrategy()).compute(2, 3).value  # 5

# Just write this:
result = 2 + 3
```

Common over-abstraction patterns to watch for:
- **Manager/Handler/Processor classes** with no state, that just call one method
- **Factory factories** — abstractions of abstractions of abstractions
- **Wrapper classes** around a single built-in with no added behavior
- **Strategy pattern** applied to something that never needs to vary

## Balancing Flexibility and Simplicity

The tension in abstraction design is between making something flexible (open to change) and keeping it simple (easy to understand). Both matter. Neither wins unconditionally.

```python
# Too rigid: hardcoded behavior, hard to extend
class ReportGenerator:
    def generate(self, data: list) -> str:
        return "\n".join(str(row) for row in data)

# Too flexible: generic machinery for a simple job
from typing import Callable

class ReportGenerator:
    def __init__(self, formatter: Callable, transformer: Callable, renderer: Callable):
        ...

# Balanced: open at the seam that's likely to change
class ReportGenerator:
    def __init__(self, formatter: Callable[[dict], str] = str):
        self.formatter = formatter

    def generate(self, data: list[dict]) -> str:
        return "\n".join(self.formatter(row) for row in data)
```

Guidelines:
- Make things flexible at the boundaries most likely to change
- Keep internals simple until there's a reason not to
- Prefer parameters over inheritance for varying behavior
- Prefer `Protocol` over `ABC` when you don't need shared implementation

## Refactoring Toward Abstractions

Good abstractions are usually discovered, not invented. Start concrete. Refactor when patterns emerge.

**Step 1: Identify repeated structure**

```python
# Before: duplicated parsing logic
def parse_user(raw: dict) -> dict:
    return {
        "id": int(raw["id"]),
        "name": raw["name"].strip(),
        "email": raw["email"].lower(),
    }

def parse_product(raw: dict) -> dict:
    return {
        "id": int(raw["id"]),
        "name": raw["name"].strip(),
        "price": float(raw["price"]),
    }
```

**Step 2: Extract the shared structure**

```python
from typing import Any

def parse_record(raw: dict, field_parsers: dict[str, Any]) -> dict:
    return {key: parser(raw[key]) for key, parser in field_parsers.items()}

USER_FIELDS = {"id": int, "name": str.strip, "email": str.lower}
PRODUCT_FIELDS = {"id": int, "name": str.strip, "price": float}

user = parse_record(raw_user, USER_FIELDS)
product = parse_record(raw_product, PRODUCT_FIELDS)
```

**Step 3: Validate the abstraction**

Ask before keeping it:
- Is this easier to read than before?
- Is this easier to test?
- Would another developer understand this without explanation?

If not — revert. A failed abstraction is not a sunk cost. Inline the logic and try again when the pattern is clearer.

>[!TIP]
>**Key takeaway:** Abstraction is a tool for managing complexity, not a goal in itself. The best abstraction is the one that makes the next change easier without making the current code harder to understand.
