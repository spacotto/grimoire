# About Conditional Statements
Conditional statements allow your code to make decisions and execute specific blocks of code based on whether a condition is **True** or **False**.

### Basic Syntax
Python uses `if`, `elif` (else if), and `else` to control logic flow:
```python
if condition:
    # Runs if condition is True
elif another_condition:
    # Runs if the first condition is False AND this one is True
else:
    # Runs if all above conditions are False
```

>[!IMPORTANT]
>**Indentation** (4 spaces) is mandatory to define the scope of each block.

>[!TIP]
>Keep nesting to a minimum. If you find yourself nesting **more than three** `if` statements deep, consider refactoring your logic for better readability.

| Operator | Description | Operator | Description|
| :--- | :--- | :--- | :--- |
| `==` | Equal to | `and` | Both must be True |
| `!=` | Not equal to | `or` | One must be True |
| `>` | Greater than | `not` | Reverse boolean |
| `<` | Less than | `in` | Membership check |

## Syntax Examples
### Standard If-Elif-Else
```python
age = 20

if age < 18:
    print("Minor")
elif age >= 18 and age < 65:
    print("Adult")
else:
    print("Senior")
```

### Ternary Operator (One-liner)
Useful for simple assignments based on a condition.
```python
status = "Access Granted" if age >= 18 else "Access Denied"
```

### Truthiness
Python evaluates "empty" objects as `False`. This allows for concise checks:
```python
items = []

if not items:
    print("The list is empty")
```
