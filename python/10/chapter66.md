# Basic Exception Handling
Exception handling lets a program respond to runtime errors gracefully instead of crashing. Python uses a try/except structure to isolate risky code, catch errors by type, recover with fallback logic, and resume normal execution — keeping programs stable and predictable.

## The `try` Block

Wraps code that *might* raise an exception. Python executes it line by line and exits immediately if an error occurs.

```python
try:
    result = 10 / 0   # this line raises ZeroDivisionError
    print(result)     # this line is skipped
```

## The `except` Block

Runs only when the `try` block raises an exception. If no exception occurs, it is skipped entirely.

```python
try:
    result = 10 / 0
except:
    print("Something went wrong.")
```

> A bare `except` catches *everything*, including system-level exceptions. Avoid it in production code.

## Catching Specific Exceptions

Name the exception type(s) to handle only what you expect. This makes errors easier to debug and avoids masking unrelated issues.

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero.")
```

**Multiple exceptions — separate handlers:**

```python
try:
    value = int(input("Enter a number: "))
    result = 10 / value
except ValueError:
    print("That's not a valid number.")
except ZeroDivisionError:
    print("Cannot divide by zero.")
```

**Multiple exceptions — single handler:**

```python
except (ValueError, ZeroDivisionError):
    print("Invalid input or division error.")
```

**Accessing the exception object:**

```python
except ValueError as e:
    print(f"Error: {e}")
```

## Basic Error Recovery

Use the `except` block to substitute a safe fallback value or prompt the user to retry.

```python
try:
    age = int(input("Enter your age: "))
except ValueError:
    age = 0   # fallback default
    print("Invalid input. Age set to 0.")
```

## Continuing Execution After Errors

Code *after* the `try/except` block runs normally regardless of whether an exception occurred, as long as it was caught.

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    result = None
    print("Division failed.")

print("Program continues...")   # always runs
print(f"Result: {result}")
```

## Exception Handling Flow

```
try block starts
    │
    ├─ No exception ──────────────────────► skip except ──► continue
    │
    └─ Exception raised
            │
            ├─ Matching except found ──────► run except ──► continue
            │
            └─ No matching except ─────────► exception propagates (crash)
```

**Key rules:**
- Only the first matching `except` block runs.
- Once an exception is caught, execution resumes *after* the entire `try/except` block.
- An unhandled exception propagates up the call stack and terminates the program.
