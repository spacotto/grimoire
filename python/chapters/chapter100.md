# Introduction to Functional Programming

Functional programming (FP) is a programming paradigm built on the idea that computation is the evaluation of mathematical functions. It avoids changing state and mutable data, promoting cleaner, more predictable code. Python, while not a purely functional language, supports many FP concepts through built-in tools and libraries.

## What is Functional Programming?

Functional programming treats functions as **first-class citizens** — they can be assigned to variables, passed as arguments, or returned from other functions.

```python
def square(x):
    return x * x

apply = square        # assign function to a variable
print(apply(5))       # 25
```

Key idea: **computation = function application**.

## Functional vs. Imperative Programming

| Aspect | Imperative | Functional |
|---|---|---|
| Focus | *How* to do it (steps) | *What* to compute (result) |
| State | Mutable, changes often | Immutable, avoided |
| Control flow | Loops, conditionals | Recursion, higher-order functions |
| Side effects | Common | Minimized |

```python
# Imperative
total = 0
for n in [1, 2, 3, 4]:
    total += n

# Functional
from functools import reduce
total = reduce(lambda a, b: a + b, [1, 2, 3, 4])
```

## Functional vs. Object-Oriented Programming

| Aspect | OOP | Functional |
|---|---|---|
| Core unit | Object (state + behavior) | Function (input → output) |
| Data | Mutable (encapsulated) | Immutable |
| Reuse | Inheritance, composition | Higher-order functions, composition |
| Side effects | Common via methods | Avoided |

FP and OOP are not mutually exclusive — Python supports both.

## Pure Functions

A **pure function**:
- Always returns the same output for the same input
- Has no side effects (no I/O, no mutation of external state)

```python
# Pure
def add(a, b):
    return a + b

# Impure (depends on external state)
total = 0
def add_to_total(x):
    global total
    total += x  # side effect!
```

Pure functions are **predictable**, **testable**, and **safe to reuse**.

## Immutability

Immutability means **data cannot be changed** after creation. Instead of modifying, you create new values.

```python
# Mutable (list)
nums = [1, 2, 3]
nums.append(4)        # mutates original

# Immutable style (tuple, or build new list)
nums = (1, 2, 3)
new_nums = nums + (4,)  # original unchanged
```

>[!TIP]
>Use `tuple`, `frozenset`, or patterns that avoid in-place mutation when possible.

## Side Effects

A **side effect** is anything a function does beyond returning a value:
- Modifying a variable outside its scope
- Writing to a file or database
- Printing to the console
- Changing global state

```python
# Side effect: modifies external list
data = []

def collect(x):
    data.append(x)  # side effect
```

FP doesn't eliminate side effects entirely — it isolates them to the edges of the program.

## Functional Programming in Python

Python provides several built-in tools for FP:

### `map()` — apply a function to each element
```python
doubled = list(map(lambda x: x * 2, [1, 2, 3]))
# [2, 4, 6]
```

### `filter()` — keep elements that match a condition
```python
evens = list(filter(lambda x: x % 2 == 0, [1, 2, 3, 4]))
# [2, 4]
```

### `reduce()` — fold a sequence into a single value
```python
from functools import reduce
product = reduce(lambda a, b: a * b, [1, 2, 3, 4])
# 24
```

### Lambda functions — anonymous, inline functions
```python
square = lambda x: x * x
print(square(6))  # 36
```

### `functools` module — tools for higher-order functions
- `partial` — fix some arguments of a function
- `lru_cache` — memoize results
- `reduce` — fold sequences

```python
from functools import partial

def power(base, exp):
    return base ** exp

square = partial(power, exp=2)
print(square(5))  # 25
```

## Benefits of Functional Programming

- **Predictability** — pure functions always behave the same way
- **Testability** — no hidden state to mock or manage
- **Parallelism** — immutable data is safe to share across threads
- **Modularity** — small, composable functions are easy to combine
- **Debugging** — fewer side effects means fewer unexpected behaviors

## When to Use Functional Programming

FP is a good fit when:
- Transforming or processing data pipelines
- Writing utility functions or data transformation logic
- You want highly testable, predictable code
- Working with concurrent or parallel systems

FP is less ideal when:
- Building stateful UIs or game logic
- Requiring heavy use of mutable data structures for performance
- Working in teams unfamiliar with FP patterns

>[!TIP]
>Python is a multi-paradigm language — use FP where it adds clarity, and OOP or imperative style where it's more natural.
