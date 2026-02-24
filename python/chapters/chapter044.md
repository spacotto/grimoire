# Generators and Iteration in Python
Generators are Python functions that produce sequences of values lazily, yielding one item at a time instead of returning all values at once. They enable memory-efficient iteration over large or infinite datasets by computing values on-demand. This guide covers generator functions, expressions, iteration protocols, and practical usage patterns.

## What are Generators?

A generator is an iterator that produces values one at a time using the `yield` keyword. Unlike regular functions that return once and terminate, generators maintain state between calls and can be resumed multiple times.

```python
def count_up_to(n):
    i = 1
    while i <= n:
        yield i
        i += 1

gen = count_up_to(3)
print(next(gen))  # 1
print(next(gen))  # 2
print(next(gen))  # 3
```

## The yield Keyword

`yield` pauses function execution and returns a value to the caller. When the generator is resumed, execution continues from where it left off.
```python
def demo():
    print("Start")
    yield 1
    print("Middle")
    yield 2
    print("End")

gen = demo()
print(next(gen))  # Prints "Start", returns 1
print(next(gen))  # Prints "Middle", returns 2
```

## Generator Functions

Any function containing `yield` becomes a generator function. Calling it returns a generator object without executing the function body.

```python
def squares(n):
    for i in range(n):
        yield i ** 2

# Function call returns generator object
gen = squares(5)

# Iterate to get values
for value in gen:
    print(value)  # 0, 1, 4, 9, 16
```

## Generator Expressions

Generator expressions provide compact syntax for creating generators, similar to list comprehensions but with parentheses.

```python
# List comprehension (creates entire list in memory)
squares_list = [x**2 for x in range(1000)]

# Generator expression (computes on demand)
squares_gen = (x**2 for x in range(1000))

# Use in iteration
print(sum(x**2 for x in range(1000)))
```

## next() and iter()

Generators implement the iterator protocol. `next()` retrieves the next value, raising `StopIteration` when exhausted. `iter()` returns the generator itself.

```python
gen = (x for x in range(3))

print(next(gen))  # 0
print(next(gen))  # 1
print(next(gen))  # 2
print(next(gen))  # StopIteration exception

# iter() returns the generator unchanged
assert iter(gen) is gen
```

## Lazy Evaluation

Generators compute values only when requested, enabling efficient processing of large datasets.

```python
def read_large_file(filepath):
    with open(filepath) as f:
        for line in f:
            yield line.strip()

# Only one line in memory at a time
for line in read_large_file('huge_file.txt'):
    process(line)
```

## Memory Efficiency with Generators

Generators use constant memory regardless of sequence length, while lists store all elements simultaneously.

```python
import sys

# List: stores all 1M integers
list_data = [x for x in range(1_000_000)]
print(sys.getsizeof(list_data))  # ~8 MB

# Generator: stores only state
gen_data = (x for x in range(1_000_000))
print(sys.getsizeof(gen_data))  # ~128 bytes
```

## Generator vs. List Performance

```python
import time

# List: immediate computation
start = time.time()
data = [x**2 for x in range(10_000_000)]
print(f"List: {time.time() - start:.2f}s")

# Generator: deferred computation
start = time.time()
data = (x**2 for x in range(10_000_000))
print(f"Generator: {time.time() - start:.6f}s")  # Nearly instant

# Actual work happens during iteration
sum(data)
```

## Infinite Generators

Generators can represent infinite sequences without consuming infinite memory.

```python
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Generate first 10 Fibonacci numbers
fib = fibonacci()
first_ten = [next(fib) for _ in range(10)]
print(first_ten)  # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

## Generator Patterns

### Pipeline Processing

```python
def read_lines(filepath):
    with open(filepath) as f:
        for line in f:
            yield line

def filter_comments(lines):
    for line in lines:
        if not line.strip().startswith('#'):
            yield line

def extract_values(lines):
    for line in lines:
        yield int(line.strip())

# Chain generators
pipeline = extract_values(filter_comments(read_lines('data.txt')))
total = sum(pipeline)
```

### Stateful Generators

```python
def moving_average(iterable, window_size):
    window = []
    for value in iterable:
        window.append(value)
        if len(window) > window_size:
            window.pop(0)
        yield sum(window) / len(window)

data = [1, 2, 3, 4, 5]
averages = list(moving_average(data, 3))
print(averages)  # [1.0, 1.5, 2.0, 3.0, 4.0]
```

## When to Use Generators

Use generators when:

- Processing large files or datasets that don't fit in memory
- Working with infinite sequences
- Building data pipelines with transformations
- Implementing lazy evaluation for expensive computations
- Only part of the sequence will be consumed

Avoid generators when:

- You need random access or multiple passes over data
- The entire dataset fits comfortably in memory and will be reused
- You need to perform operations requiring all values at once (like sorting)

## The typing.Generator Type Hint

The `Generator` type hint specifies yield type, send type, and return type.
```python
from typing import Generator

def count_up(n: int) -> Generator[int, None, None]:
    """Generator[YieldType, SendType, ReturnType]"""
    for i in range(n):
        yield i

def echo_generator() -> Generator[str, str, None]:
    """Generator that receives values via send()"""
    while True:
        received = yield "Ready"
        yield f"You sent: {received}"

def count_with_return(n: int) -> Generator[int, None, str]:
    """Generator that returns a value when exhausted"""
    for i in range(n):
        yield i
    return "Done"
```

Common patterns:

- `Generator[int, None, None]` - yields ints, no send/return
- `Generator[str, None, str]` - yields strs, returns str
- `Iterator[int]` - simpler alternative when no send/return needed
