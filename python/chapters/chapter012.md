# Function Basics
Functions are the core building blocks of reusable code in Python. This note covers how to define, name, call, and work with parameters and return values.

## What are Functions?
A **function** is a named, reusable block of code that performs a specific task. Functions help avoid repetition, improve readability, and make programs easier to maintain.

## Defining Functions with `def`
Use the `def` keyword, followed by the function name, parentheses, and a colon. The body is indented.
```python
def greet():
    print("Hello, World!")
```

## Function Naming Conventions (snake_case)
Python functions use **snake_case**: all lowercase words separated by underscores.
```python
def calculate_area():   # ✅ correct
def CalculateArea():    # ❌ avoid (PascalCase — for classes)
def calculateArea():    # ❌ avoid (camelCase)
```

- Be descriptive: `get_user_input()` is better than `gui()`
- Use verbs to reflect actions: `fetch_data()`, `parse_json()`

## Calling Functions
To execute a function, write its name followed by parentheses.
```python
greet()         # calls the function defined above
```

>[!IMPORTANT]
>A function must be **defined before it is called** (or be in scope).

## Function Parameters
Parameters are variables listed in the function definition. Arguments are the values passed when calling.
```python
def greet(name):            # `name` is a parameter
    print(f"Hello, {name}!")

greet("Alice")              # "Alice" is the argument
```

**Default parameters** provide fallback values:
```python
def greet(name="stranger"):
    print(f"Hello, {name}!")

greet()           # → Hello, stranger!
greet("Bob")      # → Hello, Bob!
```

**Multiple parameters:**
```python
def add(a, b):
    print(a + b)

add(3, 5)         # → 8
```

---

## Return Values
Use `return` to send a value back to the caller. Without it, a function returns `None`.
```python
def square(n):
    return n * n

result = square(4)   # result = 16
```

**Multiple return values** (returned as a tuple):
```python
def min_max(numbers):
    return min(numbers), max(numbers)

lo, hi = min_max([3, 1, 7, 2])   # lo=1, hi=7
```

>[!IMPORTANT]
>A function exits immediately when `return` is reached.
