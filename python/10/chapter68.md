# Multiple Exception Handling
Python allows a single `try` block to handle multiple exception types using several strategies: multiple `except` clauses, grouping exceptions in a tuple, or catching a base class. Choosing the right approach keeps error handling readable, maintainable, and precise.

## Catching Multiple Exception Types

When code can raise more than one exception, you need to account for each failure mode explicitly. Python's `try/except` construct supports this natively — no workarounds required.

## Multiple `except` Blocks

Attach one `except` clause per exception type. Each block runs independently.

```python
try:
    value = int(input("Enter a number: "))
    result = 10 / value
except ValueError:
    print("Not a valid integer.")
except ZeroDivisionError:
    print("Cannot divide by zero.")
```

>[!TIP]
>Use when each exception requires different handling logic.

## Catching Multiple Exceptions in One Block

Group exception types in a tuple to share a single handler.

```python
try:
    value = int(input("Enter a number: "))
    result = 10 / value
except (ValueError, ZeroDivisionError) as e:
    print(f"Input error: {e}")
```

>[!TIP]
>Use when multiple exceptions warrant the same response.

## Exception Handler Order

Python evaluates `except` blocks **top to bottom** and executes the **first match**. Order matters.

```python
try:
    risky_call()
except FileNotFoundError:   # specific — checked first
    print("File missing.")
except OSError:             # broader — catches remaining OS errors
    print("OS-level error.")
```

>[!NOTE]
>Placing a broad handler before a specific one makes the specific one unreachable.

## Specific vs. General Exception Handlers

| Handler | Scope | Risk |
|---|---|---|
| `except ValueError` | Catches one type | Low — explicit |
| `except (A, B)` | Catches listed types | Low — explicit |
| `except Exception` | Catches almost everything | Medium — may hide bugs |
| `except BaseException` | Catches everything incl. `SystemExit` | High — avoid |

>[!CAUTION]
>Bare `except:` (no type) is equivalent to `except BaseException` — **avoid it**.

```python
# Acceptable fallback — log, then re-raise
try:
    process()
except Exception as e:
    log_error(e)
    raise
```

## Best Practices for Multiple Handlers

- **Be specific first.** List narrow exceptions before broad ones.
- **Don't silence errors.** At minimum, log the exception before moving on.
- **Re-raise when unsure.** Use `raise` (bare) inside an `except` block to preserve the original traceback.
- **Use `as e` for context.** Binding the exception lets you log or inspect the message.
- **Keep `try` blocks small.** Wrap only the lines that can actually raise, not entire functions.
- **Avoid bare `except:`.** It swallows `KeyboardInterrupt` and `SystemExit`, making programs hard to stop.
- **Use `else` and `finally` appropriately.** `else` runs when no exception occurs; `finally` always runs (cleanup).

```python
try:
    data = fetch_data(url)
except TimeoutError:
    print("Request timed out.")
except ConnectionError:
    print("Network unreachable.")
except Exception as e:
    log_error(e)
    raise
else:
    process(data)
finally:
    close_connection()
```
