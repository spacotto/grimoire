# Basic Input and Output
Python3 provides two built-in functions for basic I/O: `print()` for displaying output and `input()` for reading user input. These are the foundation of interactive programs and console-based communication.

## The `print()` Function

`print()` writes output to the standard output (usually the terminal).
```python
print("Hello, World!")
print(42)
print(3.14)
```

**Syntax:**
```python
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```

- `sep` — separator between multiple values (default: space)
- `end` — string appended after output (default: newline)

## Basic Output Formatting

**Concatenation:**
```python
name = "Alice"
print("Hello, " + name)
```

**f-strings (recommended):**
```python
age = 30
print(f"Name: {name}, Age: {age}")
```

**`.format()` method:**
```python
print("Name: {}, Age: {}".format(name, age))
```

**Multiple values with `sep`:**
```python
print("one", "two", "three", sep=" | ")
# Output: one | two | three
```

## The `input()` Function

`input()` reads a line of text from the user and returns it as a **string**.
```python
input(prompt)
```

- `prompt` — optional string displayed before input
- Always returns a `str`, even if the user types a number

## Reading User Input
```python
name = input("Enter your name: ")
print(f"Hello, {name}!")
```

**Type conversion** is required for non-string data:
```python
age = int(input("Enter your age: "))
height = float(input("Enter your height: "))
```

**Always validate input to avoid errors:**
```python
try:
    age = int(input("Enter your age: "))
except ValueError:
    print("Invalid input. Please enter a number.")
```

## Input Prompts and User Interaction

Clear prompts improve usability. Follow these conventions:

- End prompts with `: ` or `? ` for readability.
- Confirm input back to the user when needed.
- Use loops for repeated or validated input.
```python
while True:
    try:
        number = int(input("Enter a positive number: "))
        if number > 0:
            break
        print("Must be positive. Try again.")
    except ValueError:
        print("Numbers only. Try again.")

print(f"You entered: {number}")
```

This pattern — prompt, validate, confirm — is the basis of most CLI user interactions in Python3.
