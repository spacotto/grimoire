# Built-in Exception Types
Python's exception system provides specific, named error types that identify exactly what went wrong at runtime. Knowing which exception to raise — or catch — leads to cleaner error handling, easier debugging, and more expressive code. This reference covers the most common built-in exceptions, their causes, and how to choose between them.

## ValueError — Invalid Data
Raised when an operation receives an argument of the correct type but an inappropriate value.
```python
int("abc")        # ValueError: invalid literal for int()
int("3.14")       # ValueError
range(0, 10, 0)   # ValueError: range() arg 3 must not be zero
```

## TypeError — Wrong Type of Data
Raised when an operation is applied to an object of an inappropriate type.
```python
"2" + 2           # TypeError: can only concatenate str (not "int") to str
len(42)           # TypeError: object of type 'int' has no len()
```

## ZeroDivisionError — Division by Zero
Raised when dividing (or taking modulo) by zero.
```python
10 / 0            # ZeroDivisionError: division by zero
10 % 0            # ZeroDivisionError
```

## FileNotFoundError — Missing Files
Raised when a file or directory is requested but cannot be found.
```python
open("missing.txt")  # FileNotFoundError: No such file or directory
```

## PermissionError — Access Denied
Raised when an operation lacks the required access rights.
```python
open("/etc/shadow")  # PermissionError: [Errno 13] Permission denied
```

## KeyError — Missing Dictionary Keys
Raised when a dictionary key is not found.
```python
d = {"a": 1}
d["b"]            # KeyError: 'b'
```

>[!NOTE]
>Use `d.get("b")` or `"b" in d` to avoid this.

## IndexError — List Index Out of Range
Raised when a sequence index is out of range.
```python
lst = [1, 2, 3]
lst[5]            # IndexError: list index out of range
```

## AttributeError — Missing Attributes
Raised when an attribute reference or assignment fails.
```python
x = 42
x.append(1)       # AttributeError: 'int' object has no attribute 'append'
```

## ImportError and ModuleNotFoundError
- **ImportError** — raised when an import statement fails.
- **ModuleNotFoundError** — subclass of `ImportError`; raised when the module itself cannot be found.
```python
import nonexistent_module     # ModuleNotFoundError
from os import nonexistent    # ImportError: cannot import name 'nonexistent'
```

## The Exception Hierarchy
```
BaseException
 └── Exception
      ├── ValueError
      ├── TypeError
      ├── ArithmeticError
      │    └── ZeroDivisionError
      ├── LookupError
      │    ├── KeyError
      │    └── IndexError
      ├── AttributeError
      ├── ImportError
      │    └── ModuleNotFoundError
      └── OSError
           ├── FileNotFoundError
           └── PermissionError
```
Catching a parent (e.g., `LookupError`) also catches its children (`KeyError`, `IndexError`).

## Choosing the Right Exception Type
| Situation                        | Use                    |
|----------------------------------|------------------------|
| Bad value, correct type          | `ValueError`           |
| Wrong type entirely              | `TypeError`            |
| Divide by zero                   | `ZeroDivisionError`    |
| File doesn't exist               | `FileNotFoundError`    |
| No read/write permission         | `PermissionError`      |
| Missing dict key                 | `KeyError`             |
| List/tuple index out of bounds   | `IndexError`           |
| Object lacks attribute/method    | `AttributeError`       |
| Module can't be imported         | `ModuleNotFoundError`  |

>[!TIP]
>Catch the most specific exception possible. Avoid bare `except:` — use `except Exception:` at minimum to avoid silencing `KeyboardInterrupt` and `SystemExit`.
