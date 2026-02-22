# Variables and Assignment
Variables are fundamental to programming. In Python, they store data in memory and allow you to reference, reuse, and manipulate values throughout your code. Python handles variable creation dynamically — no type declarations needed.

## Understanding Variables
A variable is a named label that points to a value stored in memory. Python is dynamically typed, meaning the type of a variable is determined at runtime, not in advance.
```python
x = 10        # x points to the integer 10
name = "Ada"  # name points to the string "Ada"
```

## Variable Assignment and the `=` Operator
The `=` operator assigns a value to a variable. It is not an equality check — it means "store this value under this name."
```python
age = 25         # assigns 25 to age
price = 9.99     # assigns a float
is_active = True # assigns a boolean
```

>[!NOTE]
>Python evaluates the right side first, then binds the result to the name on the left.

## Naming Conventions (snake_case)
Python uses `snake_case` by convention — lowercase words separated by underscores. This is defined in [PEP 8](https://peps.python.org/pep-0008/).
```python
# ✅ Preferred
user_name = "Alice"
total_price = 49.99

# ❌ Avoid
userName = "Alice"   # camelCase — not Pythonic
TotalPrice = 49.99   # PascalCase — reserved for classes
```

## Valid Variable Names
Variable names must follow these rules:

- Start with a letter (`a–z`, `A–Z`) or underscore `_`
- Contain only letters, digits (`0–9`), or underscores
- Are case-sensitive (`score` ≠ `Score`)
- Cannot be a Python keyword (`if`, `for`, `class`, etc.)
```python
# ✅ Valid
_count = 0
user1 = "Bob"
total_score = 100

# ❌ Invalid
1user = "Bob"    # starts with digit
my-var = 5       # hyphens not allowed
class = "Math"   # reserved keyword
```

## Reassigning Variables
Variables can be reassigned at any time. The old value is discarded (garbage collected if no other references exist).
```python
score = 0
score = 10      # reassigned to 10
score = score + 5  # evaluates to 15, then assigns

# Type can change on reassignment (dynamic typing)
data = 42
data = "now a string"  # valid in Python
```

>[!TIP]
>While Python allows type changes on reassignment, keeping types consistent improves readability and reduces bugs.
