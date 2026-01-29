# About `input()` Function
`input()` reads a line of text from `stdin` and returns it as a **string**.

## Basic Syntax
```python
variable = input()
variable = input(prompt)
```

## Parameters
`prompt` (optional): String displayed to the user before input is read.

## Return Value
Always returns a **string**, even if the user enters numbers.

## Examples
### Simple input
```python
name = input()
# User types: Alice
# name = "Alice"
```

### With prompt
```python
age = input("Enter your age: ")
# Displays: Enter your age: 
# User types: 25
# age = "25" (string, not int)
```

### Converting input
```python
# Convert to integer
age = int(input("Enter your age: "))

# Convert to float
price = float(input("Enter price: "))

# Multiple values on one line
x, y = input("Enter two numbers: ").split()
```

## Common Patterns
### Error handling
```python
try:
    age = int(input("Enter age: "))
except ValueError:
    print("Invalid number")
```

### Input validation
```python
while True:
    response = input("Continue? (y/n): ").lower()
    if response in ['y', 'n']:
        break
    print("Please enter y or n")
```

### Reading multiple lines
```python
lines = []
for i in range(3):
    lines.append(input())
```

## Important Notes
- `input()` always strips the trailing newline character
- In Python 2, use `raw_input()` for string input (Python 3's `input()` behavior)
- Blocks program execution until user presses Enter
- Returns empty string if user presses Enter without typing
- Raises `EOFError` if input stream ends (Ctrl+D on Unix, Ctrl+Z on Windows)

## Common Gotchas
````python
# Wrong - comparing string to int
age = input("Age: ")
if age > 18:  # TypeError: '>' not supported between str and int

# Correct
age = int(input("Age: "))
if age > 18:  # Works correctly
```
