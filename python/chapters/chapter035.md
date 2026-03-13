# Strategy Pattern

The Strategy Pattern is a behavioural design pattern that lets you swap algorithms or behaviours at runtime without changing the code that uses them. It separates *what* a program does from *how* it does it — keeping classes focused, flexible, and easy to extend.

## What is the Strategy Pattern?

The Strategy Pattern defines a family of interchangeable algorithms, encapsulates each one in its own class, and lets the client choose which to use — at runtime if needed.

Three core roles:

- **Strategy interface** — declares the method(s) all concrete strategies must implement
- **Concrete strategies** — each implements the algorithm differently
- **Context** — holds a reference to a strategy and delegates work to it

```
Client → Context → Strategy (interface)
                      ├── ConcreteStrategyA
                      ├── ConcreteStrategyB
                      └── ConcreteStrategyC
```

## Defining Strategy Interfaces

Use Python's `abc` module to define the interface. Every concrete strategy must implement the abstract method(s).

```python
from abc import ABC, abstractmethod

class SortStrategy(ABC):

    @abstractmethod
    def sort(self, data: list) -> list:
        """Sort and return the data."""
        ...
```

>[!IMPORTANT]
>**Why `ABC`?** It enforces the contract — Python raises a `TypeError` at instantiation if a subclass skips the abstract method.

## Concrete Strategy Implementations

Each concrete strategy inherits from the interface and provides its own implementation.

```python
class BubbleSortStrategy(SortStrategy):

    def sort(self, data: list) -> list:
        data = data.copy()
        n = len(data)
        for i in range(n):
            for j in range(n - i - 1):
                if data[j] > data[j + 1]:
                    data[j], data[j + 1] = data[j + 1], data[j]
        return data


class QuickSortStrategy(SortStrategy):

    def sort(self, data: list) -> list:
        if len(data) <= 1:
            return data
        pivot = data[len(data) // 2]
        left = [x for x in data if x < pivot]
        middle = [x for x in data if x == pivot]
        right = [x for x in data if x > pivot]
        return self.sort(left) + middle + self.sort(right)


class PythonSortStrategy(SortStrategy):

    def sort(self, data: list) -> list:
        return sorted(data)
```

>[!NOTE]
>Each strategy is self-contained — no cross-dependencies, no shared state.

## Context and Strategy Interaction

The Context delegates sorting to whichever strategy it holds. It does not know or care about the implementation details.

```python
class Sorter:

    def __init__(self, strategy: SortStrategy) -> None:
        self._strategy = strategy

    def sort(self, data: list) -> list:
        return self._strategy.sort(data)
```

Usage:

```python
data = [5, 3, 8, 1, 9, 2]

sorter = Sorter(QuickSortStrategy())
print(sorter.sort(data))  # [1, 2, 3, 5, 8, 9]
```

>[!NOTE]
>The Context exposes a clean interface; the algorithm lives entirely in the strategy.

## Runtime Strategy Selection

Strategies can be swapped on the fly by exposing a setter on the Context.

```python
class Sorter:

    def __init__(self, strategy: SortStrategy) -> None:
        self._strategy = strategy

    @property
    def strategy(self) -> SortStrategy:
        return self._strategy

    @strategy.setter
    def strategy(self, strategy: SortStrategy) -> None:
        self._strategy = strategy

    def sort(self, data: list) -> list:
        return self._strategy.sort(data)
```

Switching strategies at runtime:

```python
sorter = Sorter(BubbleSortStrategy())
small_result = sorter.sort([4, 2, 7])

sorter.strategy = QuickSortStrategy()   # swap at runtime
large_result = sorter.sort(list(range(10_000, 0, -1)))
```

A common pattern is to select the strategy based on input characteristics:

```python
def choose_strategy(data: list) -> SortStrategy:
    if len(data) < 20:
        return BubbleSortStrategy()
    return QuickSortStrategy()

sorter.strategy = choose_strategy(data)
```

## Strategy Composition

Strategies can be composed — a strategy can itself use other strategies or utilities, or the Context can chain multiple strategies together.

**Chaining example** — apply a filter strategy before sorting:

```python
from abc import ABC, abstractmethod

class FilterStrategy(ABC):

    @abstractmethod
    def filter(self, data: list) -> list:
        ...


class PositiveOnlyFilter(FilterStrategy):

    def filter(self, data: list) -> list:
        return [x for x in data if x > 0]


class Pipeline:

    def __init__(
        self,
        filter_strategy: FilterStrategy,
        sort_strategy: SortStrategy,
    ) -> None:
        self._filter = filter_strategy
        self._sort = sort_strategy

    def run(self, data: list) -> list:
        filtered = self._filter.filter(data)
        return self._sort.sort(filtered)
```

Usage:

```python
pipeline = Pipeline(
    filter_strategy=PositiveOnlyFilter(),
    sort_strategy=QuickSortStrategy(),
)

result = pipeline.run([-3, 5, -1, 8, 2])
print(result)  # [2, 5, 8]
```

>[!NOTE]
>Composition keeps each piece small and independently testable.

## When to Use Strategy Pattern

Use Strategy when:

- You have multiple variants of an algorithm and want to switch between them cleanly
- You want to eliminate large `if/elif` chains or `match` blocks that select behaviour
- Algorithms should be independently testable and reusable
- You anticipate adding new variants in the future without touching existing code
- You want to respect the **Open/Closed Principle** — open for extension, closed for modification

Avoid Strategy when:

- You only have one algorithm and no realistic variants planned
- The algorithm variations are trivial enough to fit cleanly in a single function with a flag parameter
- The overhead of extra classes outweighs the flexibility gain


## Benefits and Trade-offs

### Benefits

- **Flexibility.** Swap algorithms at runtime without touching the Context.
- **Separation of concerns.** Each strategy encapsulates one behaviour — single responsibility.
- **Testability.** Strategies are independent classes; easy to unit test in isolation.
- **Extensibility.** Add new strategies without modifying existing code (Open/Closed).
- **Eliminates conditionals.** Replaces branching logic with polymorphism.

### Trade-offs

- **More classes.** Each algorithm becomes its own class; small projects may not justify this.
- **Client awareness.** The client must know which strategies exist to make an informed choice.
- **Overhead.** Passing data between Context and Strategy can add indirection.

>[!TIP]
>**Rule of thumb:** If you find yourself writing `if algorithm == "x": ... elif algorithm == "y": ...` in multiple places — Strategy Pattern is likely the right fix.
