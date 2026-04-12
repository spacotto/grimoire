# First-Class Functions

In Python, functions are **first-class citizens** — they are objects like any other. This means functions can be assigned to variables, passed as arguments, returned from other functions, and stored in data structures. Mastering this concept unlocks powerful patterns: higher-order functions, callbacks, decorators, and functional programming idioms.

## Functions as Objects

Every Python function is an instance of the built-in `function` type.

```python
def greet(name):
    return f"Hello, {name}!"

print(type(greet))   # <class 'function'>
print(id(greet))     # memory address of the function object
```

Because functions are objects, they have attributes, can be passed around, and behave like any other value.

## Assigning Functions to Variables

You can bind a function to a new variable without calling it — just omit the parentheses.

```python
def square(x):
    return x ** 2

power = square          # assign, don't call
print(power(5))         # 25
print(square is power)  # True — same object
```

This is also how anonymous functions (`lambda`) work:

```python
double = lambda x: x * 2
print(double(4))  # 8
```

## Passing Functions as Arguments

Functions that receive other functions as parameters are called **higher-order functions**.

```python
def apply(func, value):
    return func(value)

def negate(x):
    return -x

print(apply(negate, 10))   # -10
print(apply(abs, -7))      # 7
```

The standard library uses this pattern extensively:

```python
nums = [3, 1, 4, 1, 5, 9]

sorted_nums = sorted(nums, key=lambda x: -x)  # sort descending
print(sorted_nums)  # [9, 5, 4, 3, 1, 1]

squares = list(map(lambda x: x ** 2, nums))
print(squares)      # [9, 1, 16, 1, 25, 81]
```

## Returning Functions from Functions

A function can build and return another function. The returned function **closes over** variables from the enclosing scope — this is called a **closure**.

```python
def make_multiplier(factor):
    def multiplier(x):
        return x * factor      # `factor` is captured from outer scope
    return multiplier          # return the function, not the result

triple = make_multiplier(3)
print(triple(7))   # 21
print(triple(10))  # 30
```

Each call to `make_multiplier` creates a fresh function with its own captured `factor`.

## Storing Functions in Data Structures

Functions can live inside lists, dictionaries, sets, or tuples — anything that holds objects.

```python
def add(a, b): return a + b
def sub(a, b): return a - b
def mul(a, b): return a * b

ops = {
    "+": add,
    "-": sub,
    "*": mul,
}

for symbol, func in ops.items():
    print(f"10 {symbol} 3 = {func(10, 3)}")

# 10 + 3 = 13
# 10 - 3 = 7
# 10 * 3 = 30
```

This pattern replaces verbose `if/elif` chains with a clean dispatch table.

## Function Attributes

Functions are objects, so you can attach arbitrary attributes to them.

```python
def process(data):
    return data.strip().lower()

process.version = "1.0"
process.author  = "Alice"

print(process.version)   # 1.0
print(process.__name__)  # process (built-in attribute)
print(process.__doc__)   # None (no docstring defined)
```

Built-in attributes on every function:

| Attribute       | Description                        |
|-----------------|------------------------------------|
| `__name__`      | The function's name as a string    |
| `__doc__`       | The docstring, or `None`           |
| `__module__`    | Module where it was defined        |
| `__defaults__`  | Tuple of default argument values   |
| `__code__`      | The compiled bytecode object       |

## `callable()` Function

Use `callable()` to check whether an object can be called with `()`.

```python
def say_hi(): return "hi"

class Greeter:
    def __call__(self):
        return "hello"

greeter = Greeter()

print(callable(say_hi))   # True
print(callable(greeter))  # True  — has __call__
print(callable(42))       # False
print(callable("hello"))  # False
```

Any object with a `__call__` method is callable — not just functions.

## Function Identity

Two function objects are the same object only if they are literally the same definition, not just equivalent code.

```python
def foo(): pass
def bar(): pass

alias = foo

print(foo is alias)  # True  — same object
print(foo is bar)    # False — different objects, even if identical body

print(foo == bar)    # False — equality falls back to identity for functions
```

Each `def` statement creates a **new** function object at runtime:

```python
funcs = [lambda x: x * i for i in range(3)]

# Common gotcha: all lambdas close over the same `i`
print([f(10) for f in funcs])  # [20, 20, 20] — i is 2 at the end of the loop

# Fix: capture the value at creation time
funcs = [lambda x, i=i: x * i for i in range(3)]
print([f(10) for f in funcs])  # [0, 10, 20]
```

## Summary

| Capability                     | Example                          |
|--------------------------------|----------------------------------|
| Assign to variable             | `f = my_func`                    |
| Pass as argument               | `apply(my_func, value)`          |
| Return from function           | `return inner_func`              |
| Store in data structure        | `{"op": my_func}`                |
| Add custom attributes          | `my_func.meta = "info"`          |
| Check callability              | `callable(obj)`                  |
| Check identity                 | `f is g`                         |

First-class functions are the foundation of decorators, callbacks, event systems, and functional programming patterns in Python. Once internalised, they make code more composable, expressive, and reusable.
