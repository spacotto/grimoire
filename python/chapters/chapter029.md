# Polymorphic Design Patterns

Polymorphism lets different objects respond to the same interface in their own way. In Python, this is achieved through inheritance, abstract base classes (`ABC`), and duck typing. These patterns share a common goal: **write code that works with types you haven't defined yet**.

## Strategy Pattern

Encapsulates interchangeable algorithms behind a common interface. Swap behaviour at runtime without changing the caller.

```python
from abc import ABC, abstractmethod

class SortStrategy(ABC):
    @abstractmethod
    def sort(self, data: list) -> list:
        ...

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

class Sorter:
    def __init__(self, strategy: SortStrategy):
        self._strategy = strategy

    def sort(self, data: list) -> list:
        return self._strategy.sort(data)

# Usage
sorter = Sorter(QuickSort())
result = sorter.sort([3, 1, 4, 1, 5])
```

>[!TIP]
>**When to use:** when you have multiple algorithms for the same task and want to switch between them cleanly.

## Template Method Pattern

Defines the skeleton of an algorithm in a base class. Subclasses fill in the steps without altering the overall structure.

```python
from abc import ABC, abstractmethod

class DataProcessor(ABC):
    def process(self, data: list) -> list:
        """Template method — fixed pipeline."""
        cleaned = self.clean(data)
        transformed = self.transform(cleaned)
        return self.format(transformed)

    @abstractmethod
    def clean(self, data: list) -> list:
        ...

    @abstractmethod
    def transform(self, data: list) -> list:
        ...

    def format(self, data: list) -> list:
        """Optional hook — override if needed."""
        return data

class CSVProcessor(DataProcessor):
    def clean(self, data: list) -> list:
        return [row.strip() for row in data if row.strip()]

    def transform(self, data: list) -> list:
        return [row.split(",") for row in data]

class JSONProcessor(DataProcessor):
    def clean(self, data: list) -> list:
        return [item for item in data if item is not None]

    def transform(self, data: list) -> list:
        import json
        return [json.dumps(item) for item in data]
```

>[!TIP]
>**When to use:** when steps of a process are fixed but their implementation varies by subclass.

## Factory Pattern with Polymorphism

Delegates object creation to a factory, so the caller works with the abstract type rather than concrete classes.

```python
from abc import ABC, abstractmethod

class Parser(ABC):
    @abstractmethod
    def parse(self, content: str) -> dict:
        ...

class JSONParser(Parser):
    def parse(self, content: str) -> dict:
        import json
        return json.loads(content)

class XMLParser(Parser):
    def parse(self, content: str) -> dict:
        # simplified
        return {"raw": content}

class ParserFactory:
    _registry: dict[str, type[Parser]] = {
        "json": JSONParser,
        "xml": XMLParser,
    }

    @classmethod
    def create(cls, format: str) -> Parser:
        parser_class = cls._registry.get(format.lower())
        if parser_class is None:
            raise ValueError(f"Unsupported format: {format}")
        return parser_class()

    @classmethod
    def register(cls, format: str, parser_class: type[Parser]) -> None:
        cls._registry[format.lower()] = parser_class

# Usage
parser = ParserFactory.create("json")
result = parser.parse('{"key": "value"}')
```

>[!TIP]
>**When to use:** when the exact type to instantiate depends on runtime input, and you want creation logic in one place.

## Adapter Pattern

Wraps an incompatible interface so it fits an expected one. Useful for integrating third-party or legacy code.

```python
from abc import ABC, abstractmethod

class DataStream(ABC):
    @abstractmethod
    def read(self) -> list[dict]:
        ...

# Legacy system with a different interface
class LegacyCSVReader:
    def get_rows(self) -> list[str]:
        return ["alice,30", "bob,25"]

# Adapter bridges LegacyCSVReader → DataStream
class CSVAdapter(DataStream):
    def __init__(self, legacy_reader: LegacyCSVReader):
        self._reader = legacy_reader

    def read(self) -> list[dict]:
        rows = self._reader.get_rows()
        return [
            {"name": parts[0], "age": int(parts[1])}
            for row in rows
            if (parts := row.split(","))
        ]

# Client code only knows DataStream
def process_stream(stream: DataStream) -> None:
    for record in stream.read():
        print(record)

legacy = LegacyCSVReader()
process_stream(CSVAdapter(legacy))
```

>[!TIP]
>**When to use:** when you need to integrate code with an incompatible interface without modifying it.

## Pipeline Pattern

Chains processing steps where each step's output feeds the next. Each step is polymorphic — the pipeline doesn't care about specifics.

```python
from abc import ABC, abstractmethod

class PipelineStep(ABC):
    @abstractmethod
    def process(self, data: list) -> list:
        ...

class FilterStep(PipelineStep):
    def __init__(self, predicate):
        self._predicate = predicate

    def process(self, data: list) -> list:
        return [item for item in data if self._predicate(item)]

class TransformStep(PipelineStep):
    def __init__(self, func):
        self._func = func

    def process(self, data: list) -> list:
        return [self._func(item) for item in data]

class Pipeline:
    def __init__(self):
        self._steps: list[PipelineStep] = []

    def add_step(self, step: PipelineStep) -> "Pipeline":
        self._steps.append(step)
        return self  # enables chaining

    def run(self, data: list) -> list:
        for step in self._steps:
            data = step.process(data)
        return data

# Usage
pipeline = (
    Pipeline()
    .add_step(FilterStep(lambda x: x > 0))
    .add_step(TransformStep(lambda x: x * 2))
)
result = pipeline.run([-1, 2, 3, -4, 5])  # [4, 6, 10]
```

>[!TIP]
>**When to use:** when data flows through a sequence of independent, reusable transformations.

## Composition with Polymorphism

Builds complex behaviour by combining simple, interchangeable components — favoring composition over inheritance.

```python
from abc import ABC, abstractmethod

class Validator(ABC):
    @abstractmethod
    def validate(self, value) -> bool:
        ...

class RangeValidator(Validator):
    def __init__(self, min_val: float, max_val: float):
        self._min = min_val
        self._max = max_val

    def validate(self, value) -> bool:
        return self._min <= value <= self._max

class TypeValidator(Validator):
    def __init__(self, expected_type: type):
        self._type = expected_type

    def validate(self, value) -> bool:
        return isinstance(value, self._type)

class CompositeValidator(Validator):
    def __init__(self, *validators: Validator):
        self._validators = list(validators)

    def validate(self, value) -> bool:
        return all(v.validate(value) for v in self._validators)

# Usage
validator = CompositeValidator(
    TypeValidator(int),
    RangeValidator(0, 100),
)
print(validator.validate(50))   # True
print(validator.validate(150))  # False
```

>[!TIP]
>**When to use:** when behaviour should be assembled from small, reusable units rather than locked into a deep inheritance tree.

## Dependency Injection

Passes dependencies into objects rather than having them create their own. Makes code testable and flexible.

```python
from abc import ABC, abstractmethod

class Logger(ABC):
    @abstractmethod
    def log(self, message: str) -> None:
        ...

class ConsoleLogger(Logger):
    def log(self, message: str) -> None:
        print(f"[LOG] {message}")

class FileLogger(Logger):
    def __init__(self, filepath: str):
        self._filepath = filepath

    def log(self, message: str) -> None:
        with open(self._filepath, "a") as f:
            f.write(f"[LOG] {message}\n")

class DataService:
    def __init__(self, logger: Logger):
        self._logger = logger  # injected, not created here

    def process(self, data: list) -> list:
        self._logger.log(f"Processing {len(data)} items")
        result = [item for item in data if item is not None]
        self._logger.log(f"Done. {len(result)} items retained.")
        return result

# Swap logger without touching DataService
service = DataService(ConsoleLogger())
service.process([1, None, 3])
```

>[!TIP]
>**When to use:** when a class needs external resources (loggers, DB connections, APIs) and you want to control or mock them from outside.

## Interface Segregation

Split large interfaces into smaller, focused ones. Classes implement only what they need.

```python
from abc import ABC, abstractmethod

# One large interface — forces irrelevant implementations
class DataHandler(ABC):
    @abstractmethod
    def read(self) -> list: ...
    @abstractmethod
    def write(self, data: list) -> None: ...
    @abstractmethod
    def delete(self) -> None: ...

# Better: segregated interfaces
class Readable(ABC):
    @abstractmethod
    def read(self) -> list: ...

class Writable(ABC):
    @abstractmethod
    def write(self, data: list) -> None: ...

class Deletable(ABC):
    @abstractmethod
    def delete(self) -> None: ...

# Each class implements only what it supports
class ReadOnlyStream(Readable):
    def read(self) -> list:
        return [1, 2, 3]

class ReadWriteStream(Readable, Writable):
    def __init__(self):
        self._data: list = []

    def read(self) -> list:
        return self._data

    def write(self, data: list) -> None:
        self._data.extend(data)

# Functions declare only the dependency they need
def load_data(source: Readable) -> list:
    return source.read()

def save_data(sink: Writable, data: list) -> None:
    sink.write(data)
```

>[!TIP]
>**When to use:** when a single interface serves too many roles, forcing implementors to define methods they don't need — a sign the interface should be split.

## Quick Reference

| Pattern | Core idea | Key benefit |
|---|---|---|
| Strategy | Swap algorithms at runtime | Flexible behavior |
| Template Method | Fix structure, vary steps | Consistent pipeline |
| Factory | Centralize object creation | Decoupled instantiation |
| Adapter | Wrap incompatible interfaces | Legacy integration |
| Pipeline | Chain processing steps | Composable transforms |
| Composition | Combine small components | Avoids deep inheritance |
| Dependency Injection | Pass in dependencies | Testability |
| Interface Segregation | Split large interfaces | Minimal coupling |
