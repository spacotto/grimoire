# The `finally` Block in Python 3
The `finally` block is a component of Python's exception handling mechanism. It guarantees that a block of code runs regardless of whether an exception occurred, making it essential for resource cleanup, connection closing, and other teardown logic.

## What is the `finally` Block?

`finally` is an optional clause that can follow `try` and `except` blocks. Its code always executes — whether the `try` block succeeded, raised an exception, or even returned early.

```python
try:
    result = 10 / 2
except ZeroDivisionError:
    print("Cannot divide by zero")
finally:
    print("This always runs")
```

## Guaranteed Cleanup with `finally`

`finally` runs in all exit scenarios:

- `try` completes normally
- An exception is raised (caught or not)
- A `return`, `break`, or `continue` is hit inside `try` or `except`

```python
def read_data():
    try:
        return "data"
    finally:
        print("Cleanup runs even before the return")
```

Output: `Cleanup runs even before the return`, then returns `"data"`.

## Resource Management

The primary use case for `finally` is releasing resources — files, sockets, database connections, locks — that must be freed regardless of success or failure.

```python
file = open("data.txt", "r")
try:
    content = file.read()
except IOError as e:
    print(f"Error reading file: {e}")
finally:
    file.close()  # Always closes, even if an exception occurred
```

## `finally` vs. `except`

| Clause   | Purpose                          | When it runs                        |
|----------|----------------------------------|-------------------------------------|
| `except` | Handle specific exceptions       | Only when a matching error occurs   |
| `finally`| Guarantee teardown/cleanup code  | Always — with or without exceptions |

They serve different roles. `except` handles errors; `finally` cleans up after them.

```python
try:
    risky_operation()
except ValueError:
    handle_value_error()   # Only on ValueError
finally:
    cleanup()              # Always
```

## When `finally` Always Executes

`finally` runs in every case **except** forced interpreter shutdown (e.g., `os._exit()`).

```python
# Case 1: No exception
try:
    x = 1 + 1
finally:
    print("Runs")  # ✓

# Case 2: Caught exception
try:
    int("abc")
except ValueError:
    pass
finally:
    print("Runs")  # ✓

# Case 3: Uncaught exception
try:
    raise RuntimeError("Oops")
finally:
    print("Runs before propagating")  # ✓

# Case 4: return inside try
def foo():
    try:
        return 42
    finally:
        print("Runs before return")  # ✓
```

## Cleanup Patterns

**Pattern 1 — No `except`, just cleanup:**
```python
try:
    do_something()
finally:
    cleanup()
```
Exceptions still propagate; the cleanup just runs first.

**Pattern 2 — Full handling + cleanup:**
```python
try:
    do_something()
except SomeError:
    handle_error()
finally:
    cleanup()
```

**Pattern 3 — `else` for success-only logic:**
```python
try:
    result = do_something()
except SomeError:
    handle_error()
else:
    process(result)   # Only runs if no exception
finally:
    cleanup()         # Always runs
```

## File and Connection Cleanup

**File handling:**
```python
f = None
try:
    f = open("data.txt")
    process(f.read())
except FileNotFoundError:
    print("File not found")
finally:
    if f:
        f.close()
```

**Database connection:**
```python
conn = get_db_connection()
try:
    conn.execute("SELECT * FROM users")
except DatabaseError as e:
    print(f"DB error: {e}")
finally:
    conn.close()  # Always release the connection
```

## Combining with Context Managers

Python's `with` statement (context managers) handles the same cleanup pattern more concisely and is preferred when available.

```python
# finally approach
f = open("data.txt")
try:
    data = f.read()
finally:
    f.close()

# Equivalent with context manager (preferred)
with open("data.txt") as f:
    data = f.read()
# f.close() called automatically
```

Use `finally` when:
- Working with resources that don't support `with`
- You need custom teardown logic beyond what a context manager provides
- Combining multiple resources with complex error handling

You can implement `__enter__` and `__exit__` (or use `contextlib`) to bring `finally`-style guarantees into reusable context managers.
