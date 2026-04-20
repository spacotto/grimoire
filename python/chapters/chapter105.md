# Closures

A closure is a function that remembers its enclosing scope even after that scope has finished executing. It binds together a function and the variables from the environment where it was defined — enabling stateful behavior without classes.

## What are Closures?

A closure is formed when an inner function references variables from its enclosing function's scope. The inner function "closes over" those variables and keeps them alive.

```python
# The inner function is the closure
def outer():
    message = "hello"
    def inner():
        print(message)  # captures 'message'
    return inner

greet = outer()
greet()  # → hello  (outer() has already returned)
```

Three conditions for a closure:
1. There is a nested function.
2. The nested function refers to a value in the enclosing scope.
3. The enclosing function returns the nested function.

## Lexical Scoping

Python uses *lexical (static) scoping*: a function's scope is determined by where it is **written** in the source, not where it is **called** from. Name resolution follows the LEGB rule.

```
LEGB: Local → Enclosing → Global → Built-in
```

```python
x = "global"

def outer():
    x = "enclosing"
    def inner():
        print(x)      # finds 'x' in Enclosing scope
    inner()

outer()  # → enclosing
```

## Nested Functions

A nested function is a function defined inside another. It can access the outer function's local variables directly. Returning the inner function creates a closure.

```python
def make_adder(n):
    def add(x):         # nested function
        return x + n    # uses 'n' from outer scope
    return add

add5 = make_adder(5)
print(add5(3))  # → 8
```

## Capturing Variables

Closures capture variables **by reference**, not by value. A classic loop pitfall: all closures share the same variable, so they all see its final value.

```python
# Pitfall — all see i=2 at call time
fns = [lambda: i for i in range(3)]
print([f() for f in fns])  # → [2, 2, 2]

# Fix — capture by value using a default argument
fns = [lambda i=i: i for i in range(3)]
print([f() for f in fns])  # → [0, 1, 2]
```

## Free Variables

A *free variable* is a variable used in a function but not defined in it. Python exposes them via `__code__.co_freevars` and the actual cell objects via `__closure__`.

```python
def outer():
    x = 10
    def inner():
        return x        # x is a free variable
    return inner

fn = outer()
print(fn.__code__.co_freevars)       # → ('x',)
print(fn.__closure__[0].cell_contents)  # → 10
```

## Closure Persistence

Captured variables persist as long as the closure exists — even after the outer function returns. Use `nonlocal` to *rebind* an enclosing variable (without it, you can read but not reassign).

```python
def make_counter():
    count = 0
    def increment():
        nonlocal count   # allows rebinding
        count += 1
        return count
    return increment

counter = make_counter()
print(counter())  # → 1
print(counter())  # → 2
print(counter())  # → 3
```

>[!NOTE]
>Without `nonlocal`, doing `count += 1` raises `UnboundLocalError` because Python treats `count` as a new local variable.

## Practical Closure Examples

### 1. Decorator

```python
def logger(fn):
    def wrapper(*args, **kwargs):
        print(f"calling {fn.__name__}")
        return fn(*args, **kwargs)
    return wrapper

@logger
def add(a, b):
    return a + b

add(2, 3)  # → calling add
```

### 2. Memoization

```python
def memoize(fn):
    cache = {}
    def wrapper(*args):
        if args not in cache:
            cache[args] = fn(*args)
        return cache[args]
    return wrapper

@memoize
def fib(n):
    return n if n < 2 else fib(n-1) + fib(n-2)
```

### 3. Partial application

```python
def multiply(x):
    return lambda n: x * n

double = multiply(2)
triple = multiply(3)
print(double(5), triple(5))  # → 10 15
```

## Closures vs. Classes

Both closures and classes encapsulate state. Closures are more concise for simple cases; classes scale better when you need multiple methods or explicit state inspection.

```python
# Closure version
def make_counter(start=0):
    count = start
    def increment():
        nonlocal count
        count += 1
        return count
    return increment

# Class version
class Counter:
    def __init__(self, start=0):
        self.count = start
    def increment(self):
        self.count += 1
        return self.count
```

| | Closure | Class |
|---|---|---|
| Syntax | Compact | Verbose |
| Multiple methods | Awkward | Natural |
| State inspection | Hard | Easy (`obj.attr`) |
| Inheritance | No | Yes |
| Pickling | Limited | Full support |

Closures implement `__call__` implicitly — a class needs to define `__call__` explicitly to make instances callable.

## When to Use Closures

**Use a closure when** you need a lightweight stateful callable without the overhead of a full class.

Good fits:
- Decorators
- Function factories
- Callbacks and event handlers
- Memoization / caching
- Partial application
- Single-operation stateful function

**Use a class instead when:**
- You need multiple related methods
- State needs to be inspected externally
- Inheritance is required
- Complex or shared state is involved
- Team readability matters more than brevity
- Serialization / pickling is needed
