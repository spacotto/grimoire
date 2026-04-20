# Higher-Order Functions in Python

Higher-order functions treat functions as first-class citizens — they can receive functions as arguments, return them as results, or both. This paradigm enables concise, reusable, and composable code by abstracting *behaviour* rather than just data.

## What are Higher-Order Functions?

A **higher-order function** is any function that:
- Takes one or more functions as arguments, **and/or**
- Returns a function as its result

In Python, functions are first-class objects: they can be assigned to variables, stored in data structures, and passed around like any other value.

```python
def greet(name):
    return f"Hello, {name}"

say_hello = greet          # assign function to variable
print(say_hello("Alice"))  # Hello, Alice
```

## Functions Taking Functions

The most common pattern: pass a function as an argument to control behaviour.

```python
def apply(func, value):
    return func(value)

def double(x):
    return x * 2

apply(double, 5)   # → 10
apply(abs, -3)     # → 3
```

Built-in examples:

```python
numbers = [3, -1, 4, -1, 5]

list(map(lambda x: x ** 2, numbers))     # → [9, 1, 16, 1, 25]
list(filter(lambda x: x > 0, numbers))  # → [3, 4, 5]

from functools import reduce
reduce(lambda acc, x: acc + x, numbers) # → 10
```

## Functions Returning Functions

A function that *builds and returns* another function — enabling dynamic behaviour and encapsulation.

```python
def multiplier(factor):
    def multiply(x):
        return x * factor
    return multiply

triple = multiplier(3)
triple(7)   # → 21
triple(10)  # → 30
```

This is the foundation of **closures**: the inner function "remembers" the enclosing scope (`factor`) even after the outer function has returned.

## Function Composition

Combining two or more functions so the output of one becomes the input of the next.

```python
def compose(f, g):
    return lambda x: f(g(x))

add_one   = lambda x: x + 1
square    = lambda x: x ** 2

square_then_add = compose(add_one, square)
square_then_add(4)   # → 17  (4² + 1)
```

For composing many functions at once:

```python
from functools import reduce

def compose_many(*funcs):
    return reduce(compose, funcs)

pipeline = compose_many(str, add_one, square)
pipeline(3)   # → "10"  (3² + 1 → "10")
```

>[!NOTE]
> `compose(f, g)` applies `g` first, then `f` — right to left, like mathematical notation.

## Function Combinators

Combinators are higher-order functions that combine or transform functions without relying on external state.

### `partial` — Fix some arguments

```python
from functools import partial

def power(base, exp):
    return base ** exp

square = partial(power, exp=2)
cube   = partial(power, exp=3)

square(5)  # → 25
cube(3)    # → 27
```

### `lru_cache` — Memoise a function

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

fib(50)  # fast — results are cached
```

### Manual combinator — `negate`

```python
def negate(predicate):
    return lambda *args, **kwargs: not predicate(*args, **kwargs)

is_even  = lambda x: x % 2 == 0
is_odd   = negate(is_even)

is_odd(3)  # → True
```

## Practical Higher-Order Functions

### `sorted()` with `key` Parameter

`sorted()` accepts a `key` function applied to each element before comparison — without modifying the original data.

```python
words = ["banana", "fig", "apple", "kiwi"]

sorted(words, key=len)               # → ['fig', 'kiwi', 'apple', 'banana']
sorted(words, key=lambda w: w[-1])   # sort by last letter
```

Sorting complex objects:

```python
people = [
    {"name": "Alice", "age": 30},
    {"name": "Bob",   "age": 25},
    {"name": "Carol", "age": 35},
]

sorted(people, key=lambda p: p["age"])
# → [Bob(25), Alice(30), Carol(35)]
```

Using `operator` for clarity and performance:

```python
from operator import itemgetter, attrgetter

sorted(people, key=itemgetter("age"))   # same result, faster
```

Multi-key sort (primary: age, secondary: name):

```python
sorted(people, key=lambda p: (p["age"], p["name"]))
```

## Custom Higher-Order Functions

### Decorator — wrapping behaviour

```python
import time

def timer(func):
    def wrapper(*args, **kwargs):
        start  = time.perf_counter()
        result = func(*args, **kwargs)
        end    = time.perf_counter()
        print(f"{func.__name__} took {end - start:.4f}s")
        return result
    return wrapper

@timer
def slow_sum(n):
    return sum(range(n))

slow_sum(1_000_000)
# slow_sum took 0.0312s
```

### Retry combinator

```python
def retry(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for attempt in range(1, times + 1):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    print(f"Attempt {attempt} failed: {e}")
            raise RuntimeError(f"All {times} attempts failed")
        return wrapper
    return decorator

@retry(times=3)
def fetch_data(url):
    ...  # may raise on network errors
```

### Pipeline builder

```python
def pipeline(*funcs):
    def execute(value):
        for func in funcs:
            value = func(value)
        return value
    return execute

process = pipeline(
    str.strip,
    str.lower,
    lambda s: s.replace(" ", "_"),
)

process("  Hello World  ")   # → "hello_world"
```

## Summary

| Concept | What it does | Key tool |
|---|---|---|
| Functions as args | Pass behavior into a function | `map`, `filter`, custom |
| Functions as return | Build specialized functions | closures, `partial` |
| Composition | Chain functions into pipelines | `compose`, `reduce` |
| Combinators | Transform/combine functions | `partial`, `lru_cache`, decorators |
| `sorted(key=)` | Control sort criteria | `key=`, `operator` module |
| Custom HOFs | Decorators, retries, pipelines | `*args`, `**kwargs`, closures |
