# Exception Handling — Best Practices
Exception handling is how your program responds to unexpected events without crashing. Done well, it makes code robust, debuggable, and user-friendly. Done poorly, it hides bugs and makes systems fragile. These notes cover the core principles for getting it right in Python 3.

## Don't Catch Everything
Avoid bare `except:` or `except Exception:` unless you have a clear, deliberate reason.
```python
# Bad — swallows every error silently
try:
    process()
except:
    pass

# Good — only catches what you expect
try:
    process()
except ValueError as e:
    handle(e)
```

>[!TIP]
>Catching everything hides bugs. If something unexpected breaks, you want to know.

## Be Specific with Exception Types
Catch the most specific exception possible. Python's exception hierarchy lets you target exactly what can go wrong.
```python
try:
    result = data[key]
except KeyError:
    result = default_value
```

>[!TIP]
>Use multiple `except` blocks when different errors need different handling. Avoid grouping unrelated exceptions together unless the response is identical.

## Fail Fast vs. Defensive Programming
**Fail fast:** Raise errors immediately when something is invalid. Better to crash early with a clear message than to propagate bad state.
```python
def set_age(age):
    if not isinstance(age, int) or age < 0:
        raise ValueError(f"Invalid age: {age}")
```

**Defensive programming:** Anticipate failures and handle them gracefully — useful at system boundaries (APIs, user input, file I/O).

>[!TIP]
>Use fail fast internally; be defensive at external boundaries.

## Logging Exceptions
Always log exceptions with enough context to reproduce the issue. Use `logging.exception()` inside an `except` block — it automatically captures the traceback.
```python
import logging

try:
    connect_to_db()
except ConnectionError as e:
    logging.exception("DB connection failed")
    raise
```

>[!TIP]
>Don't just log `str(e)` — you lose the stack trace. Avoid `print()` for error reporting in production.

## User-Friendly Error Messages
Internal exceptions and user-facing messages are different things. Translate technical errors at the boundary.
```python
try:
    load_config(path)
except FileNotFoundError:
    print(f"Config file not found: {path}. Please check the path and try again.")
```

>[!TIP]
>Never expose raw tracebacks to end users. Log the full error internally, show a clean message externally.

## Error Recovery Strategies
Common patterns:

- **Retry** — for transient failures (network, I/O). Use backoff to avoid hammering.
- **Fallback** — return a default or cached value when the primary source fails.
- **Compensating action** — undo partial work (e.g., delete a half-written file).
- **Re-raise** — catch, log, then `raise` to let the caller decide.
```python
# Retry example
for attempt in range(3):
    try:
        result = fetch_data()
        break
    except TimeoutError:
        if attempt == 2:
            raise
```

## When Not to Use Exceptions
Exceptions are for *exceptional* conditions — not normal control flow.
```python
# Bad — using exceptions for flow control
try:
    value = my_dict[key]
except KeyError:
    value = None

# Better
value = my_dict.get(key)
```

>[!TIP]
>Avoid exceptions for: expected conditions, simple validation checks, and branching logic. Use `if/else`, `.get()`, `hasattr()`, or `isinstance()` instead.

## Performance Considerations
Exception handling has overhead — primarily when an exception is *raised*, not just when a `try` block is entered.

- `try/except` with no exception raised is nearly free.
- Raising and catching exceptions in tight loops has measurable cost.
- Prefer `if` checks in hot paths where failures are *common* (not exceptional).
- Use `EAFP` (Easier to Ask Forgiveness than Permission) when failures are *rare*; use `LBYL` (Look Before You Leap) when checking is cheap and failures are frequent.
```python
# EAFP — good when key usually exists
try:
    val = d[key]
except KeyError:
    val = default

# LBYL — good when key often missing
val = d[key] if key in d else default
```
