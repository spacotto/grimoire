# Data Validation and Integrity
Data validation ensures that inputs conform to expected formats, types, ranges, and constraints before processing. Data integrity guarantees that data remains accurate and consistent throughout its lifecycle. Together, they prevent bugs, security vulnerabilities, and corrupt state. Python 3 provides built-in tools and patterns to enforce both.

## Input Validation Techniques

Validate all external input at the point of entry — never trust user input, API responses, or file data.
```python
def validate_input(value, expected_type, allowed_values=None):
    if not isinstance(value, expected_type):
        raise TypeError(f"Expected {expected_type.__name__}, got {type(value).__name__}")
    if allowed_values and value not in allowed_values:
        raise ValueError(f"Value must be one of {allowed_values}")
    return value
```
>[!TIP]
>Use a **whitelist** approach: define what is valid, reject everything else.

## Data Sanitisation

Sanitisation removes or escapes harmful/unexpected content from input before use.
```python
import html
import re

def sanitise_string(value: str) -> str:
    value = value.strip()
    value = html.escape(value)                    # Escape HTML special chars
    value = re.sub(r'[^\w\s@.\-]', '', value)     # Strip unwanted characters
    return value
```

- Strip leading/trailing whitespace
- Escape characters that could cause injection (HTML, SQL, shell)
- Normalise encoding (`str.encode('utf-8', errors='ignore').decode('utf-8')`)

## Boundary Checking

Enforce explicit limits on collection sizes, string lengths, and numeric values.
```python
def check_boundaries(value, min_val=None, max_val=None):
    if min_val is not None and value < min_val:
        raise ValueError(f"Value {value} below minimum {min_val}")
    if max_val is not None and value > max_val:
        raise ValueError(f"Value {value} exceeds maximum {max_val}")
    return value

# String length
def check_length(s: str, max_len: int) -> str:
    if len(s) > max_len:
        raise ValueError(f"String too long: {len(s)} > {max_len}")
    return s
```

>[!TIP]
>Always check list/dict sizes before iterating over untrusted input.

---

## Type Validation

Python is dynamically typed — explicit type checks prevent silent failures.
```python
# Built-in isinstance
def require_int(value) -> int:
    if not isinstance(value, int):
        raise TypeError(f"Expected int, got {type(value).__name__}")
    return value

# Using type hints + runtime enforcement via typeguard or pydantic
from pydantic import BaseModel

class UserInput(BaseModel):
    name: str
    age: int
    email: str
```

>[!NOTE]
>**Pydantic** is the standard for runtime type validation in Python 3. It raises clear errors automatically on invalid types.

## Range Validation

Ensure numeric values fall within acceptable bounds.
```python
def validate_age(age: int) -> int:
    if not (0 <= age <= 150):
        raise ValueError(f"Age out of valid range: {age}")
    return age

def validate_percentage(value: float) -> float:
    if not (0.0 <= value <= 100.0):
        raise ValueError(f"Percentage must be 0–100, got {value}")
    return value
```

Use `enum.Enum` for categorical values with a discrete valid set:
```python
from enum import Enum

class Status(Enum):
    ACTIVE = "active"
    INACTIVE = "inactive"
    PENDING = "pending"
```

## Format Validation

Validate structured strings against expected patterns using `re` or dedicated parsers.
```python
import re
from datetime import datetime

EMAIL_PATTERN = re.compile(r'^[\w.+-]+@[\w-]+\.[a-zA-Z]{2,}$')

def validate_email(email: str) -> str:
    if not EMAIL_PATTERN.match(email):
        raise ValueError(f"Invalid email format: {email}")
    return email

def validate_date(date_str: str, fmt="%Y-%m-%d") -> datetime:
    try:
        return datetime.strptime(date_str, fmt)
    except ValueError:
        raise ValueError(f"Invalid date format. Expected {fmt}")
```

>[!TIP]
>Prefer purpose-built parsers over regex for complex formats (e.g., `email-validator`, `phonenumbers`, `uuid`).

## Maintaining Data Integrity

Integrity ensures data stays valid throughout its lifecycle, not just at entry.

**1. Use immutable structures where possible**
```python
from typing import NamedTuple

class Point(NamedTuple):
    x: float
    y: float
```

**2. Validate on update, not just creation**
```python
class Account:
    def __init__(self, balance: float):
        self.balance = self._validate_balance(balance)

    def deposit(self, amount: float):
        self.balance = self._validate_balance(self.balance + amount)

    @staticmethod
    def _validate_balance(value: float) -> float:
        if value < 0:
            raise ValueError("Balance cannot be negative")
        return value
```

**3. Use database constraints** — don't rely solely on application-layer validation. Define `NOT NULL`, `CHECK`, `UNIQUE`, and foreign key constraints in the schema.

**4. Atomic operations** — use transactions to prevent partial writes:
```python
import sqlite3

with sqlite3.connect("db.sqlite3") as conn:
    conn.execute("BEGIN")
    try:
        conn.execute("UPDATE accounts SET balance = balance - ? WHERE id = ?", (100, 1))
        conn.execute("UPDATE accounts SET balance = balance + ? WHERE id = ?", (100, 2))
        conn.execute("COMMIT")
    except Exception:
        conn.execute("ROLLBACK")
        raise
```

**5. Checksums for file/data integrity**
```python
import hashlib

def file_checksum(path: str) -> str:
    h = hashlib.sha256()
    with open(path, "rb") as f:
        for chunk in iter(lambda: f.read(8192), b""):
            h.update(chunk)
    return h.hexdigest()
```

>[!IMPORTANT]
>**Key principle:** Validate early, validate completely, and enforce constraints at every layer — input, business logic, and persistence.
