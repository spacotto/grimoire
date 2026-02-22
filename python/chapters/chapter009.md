# Conditional Statements
Conditional statements allow a program to make decisions and execute different blocks of code depending on whether a condition is `True` or `False`. They are fundamental to controlling program flow.

## Making Decisions in Code
Programs often need to respond differently based on data or user input. Conditionals evaluate **boolean expressions** and branch execution accordingly. Every condition resolves to either `True` or `False`.

## The `if` Statement
Executes a block only when the condition is `True`.
```python
age = 20

if age >= 18:
    print("Adult")  # runs only if condition is True
```

- The condition is followed by a colon `:`
- The body must be **indented** (4 spaces or 1 tab)


## The `else` Clause
Provides a fallback block that runs when the `if` condition is `False`.
```python
age = 15

if age >= 18:
    print("Adult")
else:
    print("Minor")  # runs when condition is False
```

- `else` has no condition of its own
- Only one `else` is allowed per `if`

## The `elif` Statement
Short for *else if* — checks additional conditions when the previous ones are `False`.
```python
score = 72

if score >= 90:
    print("A")
elif score >= 80:
    print("B")
elif score >= 70:
    print("C")
else:
    print("F")
```

- Conditions are checked **top to bottom**; the first `True` match executes
- You can chain as many `elif` blocks as needed

## Nested Conditionals
Conditionals placed inside other conditionals. Useful for multi-layered decisions.
```python
logged_in = True
is_admin = False

if logged_in:
    if is_admin:
        print("Admin dashboard")
    else:
        print("User dashboard")
else:
    print("Please log in")
```

>[!TIP]
>Avoid deep nesting — it hurts readability. Prefer `elif` or early returns when possible.

## Boolean Logic in Conditionals
Combine conditions using logical operators: `and`, `or`, `not`.

| Operator | Returns `True` when… |
|----------|----------------------|
| `and`    | **both** conditions are `True` |
| `or`     | **at least one** condition is `True` |
| `not`    | the condition is `False` |

```python
x = 10

if x > 0 and x < 100:
    print("In range")          # both must be True

if x < 0 or x > 5:
    print("Out of lower range") # at least one True

if not x == 0:
    print("Non-zero")           # negates the condition
```

>[!IMPORTANT]
>**Short-circuit evaluation:** Python stops evaluating as soon as the result is determined — `False and ...` skips the right side; `True or ...` skips the right side.
