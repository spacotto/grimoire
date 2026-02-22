# Combining Exception Handling with File I/O
File I/O is one of the most error-prone areas in Python. Operations can fail due to missing files, permission restrictions, or underlying OS issues. Python's exception handling system provides a clean, structured way to anticipate and recover from these failures without crashing your program.

## File Operations and Error Handling

Every file operation — open, read, write, close — can raise an exception. Python's built-in
exception hierarchy covers most file-related errors under `OSError`, with more specific
subclasses for common cases.

Key exceptions to know:

- `FileNotFoundError` — file or directory does not exist
- `PermissionError` — insufficient access rights
- `IsADirectoryError` — expected a file, got a directory
- `IOError` / `OSError` — general I/O and OS-level failures

## Handling FileNotFoundError

Raised when you attempt to open or access a path that doesn't exist.
```python
try:
    with open("data.txt", "r") as f:
        content = f.read()
except FileNotFoundError:
    print("File not found. Check the path and try again.")
```

>[!TIP]
>Use this when the file's existence is uncertain — user input, config files, dynamic paths.

## Handling PermissionError

Raised when the process lacks the rights to read, write, or execute a file.
```python
try:
    with open("/etc/shadow", "r") as f:
        content = f.read()
except PermissionError:
    print("Access denied. Insufficient permissions.")
```

>[!NOTE]
>Common on protected system files or when writing to read-only locations.

## Handling IOError and OSError

`IOError` is an alias for `OSError` in Python 3. Use `OSError` to catch general I/O
failures that don't fit a more specific subclass.
```python
try:
    with open("output.txt", "w") as f:
        f.write("Hello, world!")
except OSError as e:
    print(f"OS error occurred: {e.strerror} (errno {e.errno})")
```

>[!NOTE]
>`e.errno` and `e.strerror` give you the underlying OS error code and message.

## Safe File Operations Pattern

Catch specific exceptions first, then fall back to broad ones. Always communicate failure
clearly.
```python
def read_file(path):
    try:
        with open(path, "r") as f:
            return f.read()
    except FileNotFoundError:
        print(f"No file at '{path}'.")
    except PermissionError:
        print(f"Cannot read '{path}': permission denied.")
    except OSError as e:
        print(f"Failed to read '{path}': {e.strerror}")
    return None
```

>[!NOTE]
>Returning `None` (or raising a custom exception) on failure keeps the caller in control.

## Using `with` and `try` Together

The `with` statement guarantees the file is closed — even if an exception occurs inside the
block. Combine it with `try/except` for both safety and error handling.
```python
try:
    with open("report.txt", "w") as f:
        f.write(generate_report())
except OSError as e:
    print(f"Could not write report: {e}")
```

>[!TIP]
>Never manually call `f.close()` in a `finally` block when using `with` — it's redundant.

## Crisis Response in File Systems

For operations that must not silently fail — backups, logs, config writes — make failures
loud and recoverable.
```python
import sys

def safe_write(path, data):
    try:
        with open(path, "w") as f:
            f.write(data)
    except PermissionError:
        print(f"[CRITICAL] Cannot write to '{path}': permission denied.", file=sys.stderr)
        raise  # re-raise so caller can respond
    except OSError as e:
        print(f"[CRITICAL] Write failed: {e}", file=sys.stderr)
        raise
```

>[!TIP]
>Re-raising preserves the original traceback. Use logging in production instead of `print`.

## Quick Reference

| Exception           | Cause                              | When to Catch          |
|---------------------|------------------------------------|------------------------|
| `FileNotFoundError` | Path does not exist                | Dynamic/user-defined paths |
| `PermissionError`   | Insufficient access rights         | System or protected files |
| `IsADirectoryError` | Path is a directory, not a file    | User-supplied paths    |
| `OSError`           | Any OS-level I/O failure           | General fallback       |
