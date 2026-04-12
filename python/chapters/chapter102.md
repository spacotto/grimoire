# Map, Filter, and Reduce in Python 3

## Abstract

Python provides three built-in functional programming tools — `map()`, `filter()`, and `reduce()` — that let you process iterables in a clean, expressive way. Instead of writing explicit loops, these functions apply a transformation, a condition, or an accumulation over a sequence. This document covers each function individually, then shows how they combine, and compares them to list comprehensions for context.

---

## The `map()` Function

`map(function, iterable)` applies a function to **every element** of an iterable and returns a map object (a lazy iterator).

```python
map(function, iterable)
```

- `function` — a callable applied to each element
- `iterable` — any iterable (list, tuple, generator…)

The result is **not** a list. Wrap it in `list()` to materialize it.

---

## Mapping Transformations

```python
# Square each number
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
# → [1, 4, 9, 16, 25]

# Convert strings to uppercase
words = ["hello", "world"]
upper = list(map(str.upper, words))
# → ['HELLO', 'WORLD']

# Map over multiple iterables
a = [1, 2, 3]
b = [10, 20, 30]
sums = list(map(lambda x, y: x + y, a, b))
# → [11, 22, 33]
```

> `map()` stops at the shortest iterable when given multiple iterables.

---

## The `filter()` Function

`filter(function, iterable)` keeps only elements for which the function returns `True`.

```python
filter(function, iterable)
```

- `function` — returns a boolean; elements where it's `True` are kept
- If `function` is `None`, falsy elements are removed

Like `map()`, it returns a lazy iterator.

---

## Filtering Data

```python
# Keep only even numbers
numbers = [1, 2, 3, 4, 5, 6]
evens = list(filter(lambda x: x % 2 == 0, numbers))
# → [2, 4, 6]

# Remove empty strings
words = ["apple", "", "banana", "", "cherry"]
non_empty = list(filter(None, words))
# → ['apple', 'banana', 'cherry']

# Filter objects by attribute
users = [
    {"name": "Alice", "active": True},
    {"name": "Bob",   "active": False},
    {"name": "Carol", "active": True},
]
active_users = list(filter(lambda u: u["active"], users))
# → [{'name': 'Alice', ...}, {'name': 'Carol', ...}]
```

---

## The `reduce()` Function

`reduce()` collapses an iterable into a **single value** by repeatedly applying a function to an accumulator and the next element.

```
[a, b, c, d]  →  f(f(f(a, b), c), d)
```

It is not a built-in — you must import it.

---

## `functools.reduce`

```python
from functools import reduce

reduce(function, iterable[, initializer])
```

- `function` — takes two arguments: the accumulator and the current element
- `initializer` — optional starting value (also used if the iterable is empty)

```python
from functools import reduce

# Sum of a list
total = reduce(lambda acc, x: acc + x, [1, 2, 3, 4, 5])
# → 15

# Product
product = reduce(lambda acc, x: acc * x, [1, 2, 3, 4, 5])
# → 120

# Find the maximum value manually
maximum = reduce(lambda acc, x: acc if acc > x else x, [3, 1, 4, 1, 5, 9])
# → 9

# Flatten a list of lists
nested = [[1, 2], [3, 4], [5, 6]]
flat = reduce(lambda acc, x: acc + x, nested)
# → [1, 2, 3, 4, 5, 6]
```

> Always provide an `initializer` when the iterable might be empty, to avoid a `TypeError`.

---

## Combining `map`, `filter`, `reduce`

These three functions compose naturally. A classic pattern:

1. **Filter** — keep relevant elements  
2. **Map** — transform them  
3. **Reduce** — aggregate into a result

```python
from functools import reduce

sales = [
    {"product": "A", "amount": 120, "valid": True},
    {"product": "B", "amount": 80,  "valid": False},
    {"product": "C", "amount": 200, "valid": True},
    {"product": "D", "amount": 50,  "valid": True},
]

# Total revenue from valid sales only
total = reduce(
    lambda acc, x: acc + x,
    map(
        lambda s: s["amount"],
        filter(lambda s: s["valid"], sales)
    ),
    0  # initializer
)
# → 370
```

Reading inside-out: filter valid → extract amounts → sum them.

---

## List Comprehensions vs. `map`/`filter`

For most everyday use, **list comprehensions** are preferred in Python because they are more readable.

| Use case | `map`/`filter` | List comprehension |
|---|---|---|
| Apply a named function | ✅ Clean | ✅ Also fine |
| Apply a lambda | ⚠️ Verbose | ✅ Cleaner |
| Filter + transform | ⚠️ Nested | ✅ Single expression |
| Lazy evaluation needed | ✅ Built-in | ❌ Always eager |
| Multiple iterables | ✅ Native | ⚠️ Needs `zip()` |

```python
numbers = [1, 2, 3, 4, 5, 6]

# map + filter style
result = list(map(lambda x: x ** 2, filter(lambda x: x % 2 == 0, numbers)))

# List comprehension — easier to read
result = [x ** 2 for x in numbers if x % 2 == 0]

# → [4, 16, 36]
```

Use `map()`/`filter()` when passing named functions or when you need lazy evaluation (e.g., large datasets where you don't want to build the full list in memory).

---

## Performance Considerations

| Concern | Detail |
|---|---|
| **Lazy evaluation** | `map()` and `filter()` are generators — they yield one element at a time. This is memory-efficient for large datasets. |
| **Materializing** | Wrapping in `list()` forces full evaluation. Avoid it if you only need to iterate once. |
| **`reduce()` overhead** | For simple aggregations (`sum`, `max`, `min`), prefer the dedicated built-ins — they are faster and clearer. |
| **Lambda cost** | Calling a lambda on every element carries slight overhead. A named function or a built-in method (e.g., `str.upper`) is marginally faster. |
| **List comprehensions** | Comparable speed to `map()`/`filter()` for small data; sometimes faster due to Python's internal optimization of comprehension bytecode. |

```python
# Prefer built-ins over reduce for common aggregations
total   = sum([1, 2, 3, 4, 5])       # faster than reduce
maximum = max([3, 1, 4, 1, 5, 9])    # faster than reduce
```

**Rule of thumb**: use `map()`/`filter()` for composability and lazy pipelines; use list comprehensions for clarity; use `reduce()` only when no simpler built-in covers the aggregation.
