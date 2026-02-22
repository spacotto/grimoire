# Loops and Iteration
Loops are a core control-flow mechanism in Python that allow code to execute repeatedly. This note covers the two primary loop constructs (`for` and implicit iteration), the `range()` function, loop variables, and how to iterate over common data collections.

## Understanding Repetition
Repetition in programming means executing the same block of code multiple times — either a fixed number of times or until a condition is met. Python handles this cleanly with minimal syntax, avoiding boilerplate found in other languages.

Two loop types exist:
- `for` — iterates over a sequence or iterable
- `while` — repeats while a condition is `True`

## The `for` Loop
The `for` loop iterates over any iterable object (list, string, range, etc.).

```python
for item in iterable:
    # do something with item
```

**Example:**
```python
for name in ["Alice", "Bob", "Carol"]:
    print(name)
```

- The loop body is indented (4 spaces by convention).
- The loop ends when the iterable is exhausted.

## The `range()` Function
`range()` generates a sequence of integers — useful when you need to loop a specific number of times.

| Syntax | Behavior |
|---|---|
| `range(n)` | 0 to n−1 |
| `range(start, stop)` | start to stop−1 |
| `range(start, stop, step)` | start to stop−1, stepping by step |

```python
for i in range(5):        # 0, 1, 2, 3, 4
    print(i)

for i in range(2, 10, 2): # 2, 4, 6, 8
    print(i)
```

>[!NOTE]
>`range()` is memory-efficient — it generates values on demand, not all at once.

## Loop Variables and Counters
The loop variable takes the value of each element in the iterable on each iteration. It can be named anything, but should be descriptive.

Use `_` as a throwaway variable when the value isn't needed:
```python
for _ in range(3):
    print("repeat")
```

**Manual counter** (when you need an index):
```python
count = 0
for item in items:
    count += 1
```

**Preferred: `enumerate()`** — gives index and value together:
```python
for i, item in enumerate(items):
    print(i, item)
```

## Iterating Through Collections

### List
```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```

### String
```python
for char in "hello":
    print(char)  # h, e, l, l, o
```

### Dictionary
```python
data = {"a": 1, "b": 2}

for key in data:            # keys only
for key, val in data.items(): # key-value pairs
for val in data.values():   # values only
```

### Tuple
```python
coords = (10, 20, 30)
for n in coords:
    print(n)
```

### Nested Loops
```python
for row in matrix:
    for cell in row:
        print(cell)
```

>[!NOTE]
>Each inner loop completes fully before the outer loop advances.
