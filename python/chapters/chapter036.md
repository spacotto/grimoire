# Combining Design Patterns

Design patterns rarely work in isolation. In real-world software, you'll often combine two or more patterns to solve complex problems cleanly. This guide covers how to compose patterns together in Python 3, when it makes sense, and how to avoid the trap of over-engineering.

## Factory + Strategy Combination

Use a **Factory** to create **Strategy** objects dynamically. The factory decides which strategy to instantiate; the strategy defines the behavior.

```python
from abc import ABC, abstractmethod

# Strategy interface
class SortStrategy(ABC):
    @abstractmethod
    def sort(self, data: list) -> list:
        pass

class BubbleSort(SortStrategy):
    def sort(self, data: list) -> list:
        data = data[:]
        for i in range(len(data)):
            for j in range(len(data) - i - 1):
                if data[j] > data[j + 1]:
                    data[j], data[j + 1] = data[j + 1], data[j]
        return data

class QuickSort(SortStrategy):
    def sort(self, data: list) -> list:
        if len(data) <= 1:
            return data
        pivot = data[len(data) // 2]
        left = [x for x in data if x < pivot]
        middle = [x for x in data if x == pivot]
        right = [x for x in data if x > pivot]
        return self.sort(left) + middle + self.sort(right)

# Factory
class SortStrategyFactory:
    _strategies = {
        "bubble": BubbleSort,
        "quick": QuickSort,
    }

    @classmethod
    def create(cls, name: str) -> SortStrategy:
        strategy_class = cls._strategies.get(name)
        if not strategy_class:
            raise ValueError(f"Unknown strategy: {name}")
        return strategy_class()

# Usage
strategy = SortStrategyFactory.create("quick")
result = strategy.sort([5, 3, 1, 4, 2])
print(result)  # [1, 2, 3, 4, 5]
```

>[!TIP]
>**When to use it:** When the algorithm to apply is determined at runtime and you want to centralize instantiation logic.

## Abstract Factory + Template Method

Use an **Abstract Factory** to produce families of related objects, each of which follows a **Template Method** to define a common execution flow.

```python
from abc import ABC, abstractmethod

# Template Method base
class ReportGenerator(ABC):
    def generate(self, data: dict) -> str:
        header = self.build_header(data)
        body = self.build_body(data)
        footer = self.build_footer(data)
        return f"{header}\n{body}\n{footer}"

    @abstractmethod
    def build_header(self, data: dict) -> str: pass

    @abstractmethod
    def build_body(self, data: dict) -> str: pass

    def build_footer(self, data: dict) -> str:
        return "--- End of Report ---"

# Concrete implementations
class HTMLReport(ReportGenerator):
    def build_header(self, data: dict) -> str:
        return f"<h1>{data['title']}</h1>"

    def build_body(self, data: dict) -> str:
        return f"<p>{data['content']}</p>"

class MarkdownReport(ReportGenerator):
    def build_header(self, data: dict) -> str:
        return f"# {data['title']}"

    def build_body(self, data: dict) -> str:
        return data['content']

# Abstract Factory
class ReportFactory(ABC):
    @abstractmethod
    def create_report(self) -> ReportGenerator:
        pass

class HTMLReportFactory(ReportFactory):
    def create_report(self) -> ReportGenerator:
        return HTMLReport()

class MarkdownReportFactory(ReportFactory):
    def create_report(self) -> ReportGenerator:
        return MarkdownReport()

# Usage
factory = MarkdownReportFactory()
report = factory.create_report()
output = report.generate({"title": "Summary", "content": "All systems nominal."})
print(output)
```

>[!TIP]
>**When to use it:** When you need to swap entire families of objects (e.g., HTML vs. Markdown output) while keeping a consistent processing skeleton.

## Strategy + Decorator Patterns

Use **Decorator** to add cross-cutting behavior (logging, caching, validation) to a **Strategy** without modifying it.

```python
from abc import ABC, abstractmethod
import time

class PricingStrategy(ABC):
    @abstractmethod
    def calculate(self, amount: float) -> float:
        pass

class StandardPricing(PricingStrategy):
    def calculate(self, amount: float) -> float:
        return amount

class DiscountPricing(PricingStrategy):
    def calculate(self, amount: float) -> float:
        return amount * 0.9

# Decorator base — wraps a strategy
class PricingDecorator(PricingStrategy):
    def __init__(self, strategy: PricingStrategy):
        self._strategy = strategy

    def calculate(self, amount: float) -> float:
        return self._strategy.calculate(amount)

# Concrete decorators
class LoggingDecorator(PricingDecorator):
    def calculate(self, amount: float) -> float:
        result = self._strategy.calculate(amount)
        print(f"[LOG] Input: {amount:.2f} → Output: {result:.2f}")
        return result

class TimingDecorator(PricingDecorator):
    def calculate(self, amount: float) -> float:
        start = time.perf_counter()
        result = self._strategy.calculate(amount)
        elapsed = time.perf_counter() - start
        print(f"[TIMING] Took {elapsed:.6f}s")
        return result

# Usage — stack decorators
strategy = TimingDecorator(LoggingDecorator(DiscountPricing()))
strategy.calculate(100.0)
# [LOG] Input: 100.00 → Output: 90.00
# [TIMING] Took 0.000012s
```

>[!TIP]
>**When to use it:** When you want to add observability or validation around interchangeable algorithms without coupling them together.

## Composing Multiple Patterns

Patterns can be layered. Here's a pipeline that uses **Factory**, **Strategy**, and **Observer** together.

```python
from abc import ABC, abstractmethod
from typing import Callable

# Observer
class EventBus:
    def __init__(self):
        self._listeners: dict[str, list[Callable]] = {}

    def subscribe(self, event: str, callback: Callable) -> None:
        self._listeners.setdefault(event, []).append(callback)

    def emit(self, event: str, payload=None) -> None:
        for callback in self._listeners.get(event, []):
            callback(payload)

# Strategy
class DataProcessor(ABC):
    @abstractmethod
    def process(self, data: list) -> list:
        pass

class FilterEven(DataProcessor):
    def process(self, data: list) -> list:
        return [x for x in data if x % 2 == 0]

class DoubleValues(DataProcessor):
    def process(self, data: list) -> list:
        return [x * 2 for x in data]

# Factory
class ProcessorFactory:
    _map = {"filter_even": FilterEven, "double": DoubleValues}

    @classmethod
    def create(cls, name: str) -> DataProcessor:
        cls_ = cls._map.get(name)
        if not cls_:
            raise ValueError(f"Unknown processor: {name}")
        return cls_()

# Pipeline — composes all three
class Pipeline:
    def __init__(self, bus: EventBus):
        self._steps: list[DataProcessor] = []
        self._bus = bus

    def add_step(self, processor_name: str) -> "Pipeline":
        self._steps.append(ProcessorFactory.create(processor_name))
        return self

    def run(self, data: list) -> list:
        self._bus.emit("pipeline.start", data)
        result = data
        for step in self._steps:
            result = step.process(result)
            self._bus.emit("step.complete", result)
        self._bus.emit("pipeline.end", result)
        return result

# Usage
bus = EventBus()
bus.subscribe("pipeline.end", lambda r: print(f"Final result: {r}"))

pipeline = Pipeline(bus)
pipeline.add_step("filter_even").add_step("double")
pipeline.run([1, 2, 3, 4, 5, 6])
# Final result: [4, 8, 12]
```

## Pattern Interaction and Communication

When patterns interact, define clear boundaries to avoid tight coupling.

| Concern | Recommendation |
|---|---|
| Data sharing | Pass data explicitly via method arguments, not shared state |
| Event flow | Use an event bus or callback list to decouple emitters from listeners |
| Object creation | Centralize in factories; avoid `new` calls scattered across business logic |
| Behavior extension | Prefer decorators or composition over subclassing |

>[!TIP]
>**Key principle:** Each pattern should have one clear responsibility. When two patterns need to communicate, do it through a well-defined interface — not by reaching into each other's internals.

## Avoiding Over-Engineering

More patterns ≠ better code. Ask yourself:

- **Does this solve a real, current problem?** Don't add a factory if you only ever create one type.
- **Would a simpler structure work?** A plain function or class is often enough.
- **Is the pattern visible to future readers?** If you need extensive comments to explain why the pattern is there, reconsider.
- **Are you solving for flexibility you actually need?** Premature abstraction is as costly as premature optimization.

```python
# Over-engineered
class GreeterFactory:
    def create(self) -> "Greeter":
        return Greeter()

class Greeter:
    def greet(self, name: str) -> str:
        return f"Hello, {name}"

# Just right
def greet(name: str) -> str:
    return f"Hello, {name}"
```

>[!TIP]
>Introduce patterns when you feel the pain of **not** having them — not before.

## Practical Pattern Combinations

| Combination | Use Case |
|---|---|
| Factory + Strategy | Runtime algorithm selection (parsers, sorters, formatters) |
| Abstract Factory + Template Method | Platform-specific output with consistent structure |
| Strategy + Decorator | Algorithms with optional cross-cutting concerns (logging, caching) |
| Observer + Command | Undo/redo systems, event-driven UIs |
| Composite + Iterator | Tree traversal (file systems, ASTs, UI component trees) |
| Builder + Factory | Complex object construction with multiple valid configurations |

## Real-World Pattern Usage

### Web Framework Request Handling

```python
# Strategy: auth method (JWT, OAuth, API key)
# Decorator: rate limiting, logging
# Factory: middleware stack construction

class AuthStrategy(ABC):
    @abstractmethod
    def authenticate(self, token: str) -> bool:
        pass

class JWTAuth(AuthStrategy):
    def authenticate(self, token: str) -> bool:
        return token.startswith("eyJ")  # simplified

class MiddlewareDecorator(AuthStrategy):
    def __init__(self, strategy: AuthStrategy, rate_limit: int):
        self._strategy = strategy
        self._rate_limit = rate_limit
        self._call_count = 0

    def authenticate(self, token: str) -> bool:
        self._call_count += 1
        if self._call_count > self._rate_limit:
            raise RuntimeError("Rate limit exceeded")
        return self._strategy.authenticate(token)

auth = MiddlewareDecorator(JWTAuth(), rate_limit=100)
print(auth.authenticate("eyJhbGci..."))  # True
```

### Data Export Pipeline

```python
# Template Method: defines export steps
# Strategy: output format (CSV, JSON, XML)
# Factory: creates the right exporter

import json
import csv
import io

class Exporter(ABC):
    def export(self, records: list[dict]) -> str:
        data = self.serialize(records)
        return self.wrap(data)

    @abstractmethod
    def serialize(self, records: list[dict]) -> str:
        pass

    def wrap(self, data: str) -> str:
        return data  # override if needed

class JSONExporter(Exporter):
    def serialize(self, records: list[dict]) -> str:
        return json.dumps(records, indent=2)

class CSVExporter(Exporter):
    def serialize(self, records: list[dict]) -> str:
        if not records:
            return ""
        buffer = io.StringIO()
        writer = csv.DictWriter(buffer, fieldnames=records[0].keys())
        writer.writeheader()
        writer.writerows(records)
        return buffer.getvalue()

class ExporterFactory:
    _formats = {"json": JSONExporter, "csv": CSVExporter}

    @classmethod
    def create(cls, fmt: str) -> Exporter:
        cls_ = cls._formats.get(fmt)
        if not cls_:
            raise ValueError(f"Unsupported format: {fmt}")
        return cls_()

# Usage
records = [{"name": "Alice", "score": 95}, {"name": "Bob", "score": 87}]
for fmt in ("json", "csv"):
    exporter = ExporterFactory.create(fmt)
    print(f"--- {fmt.upper()} ---")
    print(exporter.export(records))
```

>[!TIP]
>**Rule of thumb:** Start simple. Introduce a pattern when your code starts to hurt — not when you think it might hurt someday.
