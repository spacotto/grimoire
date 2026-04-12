# Lambda Expressions in Python

Lambda expressions are small, anonymous functions defined inline using the `lambda` keyword. They're useful for short, throwaway operations — especially when passing functions as arguments. This document covers syntax, use cases, limitations, and best practices.

## What Are Lambda Functions?

A **lambda function** is a function without a name, defined in a single expression. Like regular functions, lambdas accept arguments and return a value — but they're written in one line and need no `def` or `return` statement.

```python
# A regular function
def square(x):
    return x * x

# The equivalent lambda
square = lambda x: x * x
```

## Lambda Syntax

```
lambda <arguments> : <expression>
```

- `lambda` — keyword that starts the definition
- `<arguments>` — zero or more comma-separated parameters
- `<expression>` — a single expression whose result is returned automatically

```python
# No arguments
greet = lambda: "Hello!"

# One argument
double = lambda x: x * 2

# Multiple arguments
add = lambda x, y: x + y

# With default values
power = lambda x, exp=2: x ** exp
```

## Anonymous Functions

Lambdas are called **anonymous** because they don't require a name. Their most common use is being passed directly — inline — without ever being assigned to a variable.

```python
# Passed directly, never assigned
result = (lambda x, y: x + y)(3, 5)  # → 8
```

This is valid, but naming a lambda (as above) is usually better handled with `def`.

## Lambda vs. `def`

| Feature              | `lambda`               | `def`                    |
|----------------------|------------------------|--------------------------|
| Name                 | Optional / anonymous   | Required                 |
| Body                 | Single expression only | Any number of statements |
| Return               | Implicit               | Explicit (`return`)      |
| Docstrings           | ✗ Not supported        | ✓ Supported              |
| Readability          | Good for simple ops    | Better for complex logic |

>[!TIP]
>**Rule of thumb:** if a lambda needs more than one line or a description, use `def`.

## When to Use Lambda

Lambdas shine when:

- You need a **short, one-off function** as an argument
- You don't want to clutter the namespace with a named function
- The logic is **simple enough to read at a glance**

```python
# Sorting with a custom key
students = [("Alice", 90), ("Bob", 75), ("Carol", 85)]
students.sort(key=lambda s: s[1])

# Quick conditional logic
classify = lambda x: "even" if x % 2 == 0 else "odd"
```

## Lambda Limitations

Lambdas are intentionally restricted:

- **Single expression only** — no multiple statements, no assignments
- **No `return` keyword** — the expression result is returned automatically
- **No docstrings** — can't document them inline
- **No type annotations** — reduces IDE support and clarity
- **Harder to debug** — tracebacks show `<lambda>` instead of a meaningful name

```python
# This is invalid — lambdas can't contain statements
broken = lambda x: if x > 0: return x  # SyntaxError
```

## Lambda with Built-in Functions

The most common and idiomatic use of lambdas is with Python's higher-order built-ins.

```python
numbers = [4, 1, 7, 2, 9, 3]

# sorted() — custom sort key
sorted_nums = sorted(numbers, key=lambda x: -x)  # descending

# map() — apply a function to each element
squares = list(map(lambda x: x ** 2, numbers))

# filter() — keep elements that satisfy a condition
evens = list(filter(lambda x: x % 2 == 0, numbers))
```

> [!NOTE]
> List comprehensions are often more readable than `map()` and `filter()` with lambdas:
> ```python
> squares = [x ** 2 for x in numbers]  # preferred
> evens   = [x for x in numbers if x % 2 == 0]  # preferred
> ```

## Multi-line Lambdas (Avoiding)

Python does **not** support multi-line lambdas — and that's by design. If you need more than one expression, use `def`:

```python
# Tempting but wrong — don't do this
process = lambda x: (
    x * 2 if x > 0
    else -x
)

# Better — readable, debuggable, documentable
def process(x):
    """Return double for positives, absolute value otherwise."""
    if x > 0:
        return x * 2
    return -x
```
