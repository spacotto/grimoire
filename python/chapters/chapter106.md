# The `nonlocal` Keyword

The `nonlocal` keyword lets an inner function *write to* a variable defined in an enclosing (but non-global) scope. Without it, assignment always creates a new local variable, leaving the outer one untouched.

## Understanding `nonlocal`

Python resolves names using the **LEGB** rule: Local → Enclosing → Global → Built-in. Reading an outer variable works automatically. *Assigning* to one requires an explicit declaration.

```python
def outer():
    x = 10
    def inner():
        # x is readable here, but assigning creates a new local x
        print(x)   # OK — reads from enclosing scope
    inner()

outer()  # → 10
```

## Modifying Enclosing Scope

Declare `nonlocal` before the assignment to target the enclosing variable instead of creating a new local one.

```python
def outer():
    x = 10
    def inner():
        nonlocal x
        x = 20       # modifies outer's x
    inner()
    print(x)         # → 20

outer()
```

>[!NOTE]
>The variable must already exist in an enclosing (non-global) scope, or Python raises a `SyntaxError`.

## `nonlocal` vs. `global`

| keyword    | targets                              | creates if missing? |
|------------|--------------------------------------|---------------------|
| `global`   | module-level variable                | yes                 |
| `nonlocal` | nearest enclosing function variable  | no — must exist     |

```python
# global example
count = 0
def increment():
    global count
    count += 1

# nonlocal example
def outer():
    count = 0
    def increment():
        nonlocal count
        count += 1
    increment()
    return count  # → 1
```

## Stateful Closures

A closure is a function that *captures* its enclosing scope. Combined with `nonlocal`, the captured variable persists state across calls — without a class.

## Counter Closures

```python
def make_counter(start=0):
    count = start
    def counter():
        nonlocal count
        count += 1
        return count
    return counter

c1 = make_counter()
c2 = make_counter(10)

print(c1())  # → 1
print(c1())  # → 2
print(c2())  # → 11  — independent state
```

Each call to `make_counter()` produces an independent counter with its own `count` variable.

## Accumulator Patterns

```python
def make_accumulator():
    total = 0.0
    history = []
    def add(value):
        nonlocal total
        total += value
        history.append(value)
        return total
    def reset():
        nonlocal total
        total = 0.0
        history.clear()
    add.reset = reset
    add.history = history
    return add

acc = make_accumulator()
print(acc(10))  # → 10.0
print(acc(5))   # → 15.0
acc.reset()
print(acc(3))   # → 3.0
```

>[!NOTE]
>Mutable objects like `history` (a list) don't need `nonlocal` for mutation — only for *rebinding* the name.

## Best Practices with `nonlocal`

**Do:**
- Use `nonlocal` in short, focused closures where the stateful pattern is clear.
- Prefer it over `global` to limit side-effect scope.
- Use it for factory functions (counters, accumulators, validators) that return callables.

**Avoid:**
- Deep nesting with multiple `nonlocal` declarations — it becomes hard to trace which scope owns the variable.
- Using `nonlocal` when a class would be clearer (complex state with many methods).
- Rebinding mutable objects unnecessarily — mutate them in place instead.

```python
# avoid: rebinding a list (nonlocal needed but surprising)
def bad():
    items = []
    def reset():
        nonlocal items
        items = []       # creates a new list object
    return reset

# prefer: mutate in place (no nonlocal needed)
def good():
    items = []
    def reset():
        items.clear()    # mutates the existing object
    return reset
```
