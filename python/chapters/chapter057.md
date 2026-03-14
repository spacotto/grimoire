# Breaking Circular Dependencies

A **circular dependency** occurs when module A imports module B, and module B imports module A — directly or through a chain. Python raises an `ImportError` or returns a partially-initialised module, leading to subtle, hard-to-debug failures.

```
# Classic circular import problem
# file: a.py
from b import B       # triggers b.py to load

# file: b.py
from a import A       # a.py is still loading → ImportError
```

Circular dependencies are a design signal: they usually mean responsibilities are tangled and modules are too tightly coupled. The solutions below range from quick fixes to structural refactors.

## Late Imports (Import Inside Functions)

Move the import inside the function or method that needs it, instead of placing it at the top of the file. Python caches modules after the first load, so the performance cost is minimal.

```python
# file: a.py
class A:
    def get_b(self):
        from b import B   # imported only when this method runs
        return B()
```

>[!TIP]
>**When to use:** quick fix for a single, non-critical dependency.  

>[!WARNING]
>**Avoid:** scattering late imports everywhere — they hide dependencies and hurt readability.

## Dependency Injection

Instead of importing a class or function directly, pass it in as a parameter. The module no longer needs to know where the dependency comes from.

```python
# file: processor.py
class Processor:
    def __init__(self, formatter):   # formatter injected, not imported
        self.formatter = formatter

    def run(self, data):
        return self.formatter.format(data)
```

```python
# file: main.py
from formatter import Formatter
from processor import Processor

p = Processor(formatter=Formatter())
p.run(data)
```

>[!TIP]
>**When to use:** when two classes depend on each other's *behavior*, not on their *definition*.  

>[!NOTE]
>**Benefit:** also improves testability (easy to inject mocks).

## Shared/Common Modules

Extract the shared type, constant, or utility that both modules need into a third module. Neither of the original modules imports the other — both import the shared one.

```python
# file: models.py  ← shared module, imports nothing from a.py or b.py
class Record:
    def __init__(self, value):
        self.value = value
```

```python
# file: a.py
from models import Record   # no import of b.py

# file: b.py
from models import Record   # no import of a.py
```

>[!TIP]
>**When to use:** when the circular dependency is caused by a shared data structure or constant.  

>[!NOTE]
>**Common pattern:** a `models.py`, `types.py`, `constants.py`, or `enums.py` module with no business logic.

## Restructuring Code

Sometimes the circular dependency is a sign that two modules belong together, or that one module is doing too much. Options:

- **Merge** the two modules into one.
- **Split** one module into smaller, single-purpose pieces.
- **Move** the function or class causing the cycle to the module that depends on it.

```python
# Before: writer.py imports reader.py, reader.py imports writer.py
# After: merge both into io_handler.py, or extract the shared logic into buffer.py
```

>[!TIP]
>**When to use:** when the circular dependency reflects a genuine design problem, not just an import order issue.

## Interface Modules

Define an abstract interface (using `abc`) in a separate module. Concrete implementations import the interface; callers depend on the interface, not the implementation.

```python
# file: interfaces.py
from abc import ABC, abstractmethod

class BaseFormatter(ABC):
    @abstractmethod
    def format(self, data: str) -> str:
        ...
```

```python
# file: csv_formatter.py
from interfaces import BaseFormatter   # no circular reference

class CSVFormatter(BaseFormatter):
    def format(self, data: str) -> str:
        ...
```

```python
# file: processor.py
from interfaces import BaseFormatter   # depends on abstraction, not implementation

class Processor:
    def __init__(self, formatter: BaseFormatter):
        self.formatter = formatter
```

>[!TIP]
>**When to use:** when modules depend on behavior that should be interchangeable (e.g., multiple formatters, parsers, adapters).  

>[!NOTE]
>**Benefit:** enables the Open/Closed Principle — add new implementations without touching existing modules.

## Choosing the Right Solution

| Situation | Recommended approach |
|---|---|
| One-off, low-impact dependency | Late import |
| Two classes depend on each other's behavior | Dependency injection |
| Shared type/constant causes the cycle | Shared/common module |
| Modules are genuinely entangled | Restructure (merge or split) |
| Many modules depend on one behavior | Interface module |

Start with the simplest fix. If the problem recurs or the architecture feels forced, escalate to restructuring or interfaces.

## Prevention Strategies

- **Draw your dependency graph.** Modules should form a directed acyclic graph (DAG). Tools like `pydeps` or `importlab` can visualize it.
- **Layer your architecture.** Low-level modules (models, utils, constants) never import from high-level ones (services, controllers). Dependencies flow in one direction only.
- **One responsibility per module.** Modules that do one thing are less likely to need a back-reference.
- **Prefer passing objects over importing them.** When in doubt, inject rather than import.
- **Review imports at PR time.** Circular imports introduced incrementally are hard to notice without a checklist.

```
# Healthy layered dependency direction:
controllers → services → repositories → models → (nothing)
```

## Design Patterns to Avoid Circularity

| Pattern | How it helps |
|---|---|
| **Dependency Injection** | Decouples instantiation from usage; no back-imports needed |
| **Observer / Event Bus** | Module A emits an event; module B subscribes — neither imports the other |
| **Strategy** | Behavior is passed in, not imported; the host module stays agnostic |
| **Factory** | Object creation is centralized; downstream modules only import the factory's output type |
| **Service Locator** | A registry resolves dependencies at runtime; modules register themselves, not each other |

```python
# Observer example: no direct import between emitter and listener
# file: events.py  ← the shared bus, imported by both
handlers = {}

def subscribe(event, fn):
    handlers.setdefault(event, []).append(fn)

def emit(event, payload):
    for fn in handlers.get(event, []):
        fn(payload)
```

```python
# file: a.py
from events import emit

def do_something():
    emit("record_created", {"id": 42})   # no import of b.py
```

```python
# file: b.py
from events import subscribe

def on_record_created(payload):
    print(f"New record: {payload['id']}")

subscribe("record_created", on_record_created)   # no import of a.py
```

>[!TIP]
>**Rule of thumb:** if you find yourself reaching for a late import as a fix, treat it as a warning sign. Use it to unblock yourself, then schedule a refactor.
