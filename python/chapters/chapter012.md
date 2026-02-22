# Function Basics
Functions are the core building blocks of reusable code in Python. This note covers how to define, name, call, and work with parameters and return values.

## What are Functions?
A **function** is a named, reusable block of code that performs a specific task. Functions help avoid repetition, improve readability, and make programs easier to maintain.

Basic structure:
```python
def function_name(parameters):
    # function body
    return #optional
```

### Anatomy of a Function
A function consists of several elements, some mandatory (they must always be present), some optional.
1. **The header**, the first **line that defines the function’s interface**.
2. **The scope**, the body where the instructions take place.

The header itself is composed of several elements:

| Element  | Syntax   | Function | Presence |
| :------- | :------- | :------- | :------- | 
| Defining keyword | `def` | Marks the start of the definition | Mandatory |
| Function name | `snake_case` | A unique identifier | Mandatory |
| Parameters | `(snake_case)` | Variables listed inside parentheses `()` that receive input values (arguments) | Optional |
| Function Signature | `-> ...` | The exptected data to return | Optional |
| Colon | `:` | Terminates the header and initiates the code block | Mandatory |

>[!IMPORTANT]
>**Type hinting** is a **formal syntax** used to **specify the expected data types** of function **arguments** and **return values**. For example:
>```python
>def greet(name: str, age: int) -> str:
>    return f"Hello {name}, you are {age} years old."
>```
>Type hinting **reduces bugs**, catching `TypeErrors` before you run the script, your IDE provides better suggestions (**autocomplete**) because it knows the object type, and helps writing **cleaner code** by replacing the need for comments describing variable types.


## Defining Functions with `def`
Use the `def` keyword, followed by the function name, parentheses, and a colon. The body is indented.

```python
def greet():
    print("Hello, World!")
```

## Function Naming

A **function name** is a **unique identifier** used to reference and execute a specific block of code.

### Naming Rules (Mandatory)
Python enforces strict **rules** for identifiers. If these aren't followed, the code will throw a `SyntaxError`.
The function name...
- MUST **start** with a **letter** (`a-z`, `A-Z`) or an **underscore** (`_`).
- can only contain **alphanumeric** characters and **underscores** (`a-z`, `0-9`, and `_`).
- is **case sensitive**: `my_function()` and `My_Function()` are treated as two different functions.
- CANNOT use **Python keywords** (like `def`, `if`, `while`, or `class`) as names.

### Naming Conventions (Best Practices)
While the rules prevent errors, conventions (specifically PEP 8) ensure your code is readable:
- `snake_case`: Use lowercase words separated by underscores (e.g., `calculate_user_age`).
- **Descriptive**: The name should be a verb or a phrase indicating what the function does (e.g., `get_data`).
- **Avoid Shadows**: Do NOT name your function after built-ins like print or list, as this will "hide" the original Python functionality.

### The Function Name as a Reference
In Python, the name is actually a variable that points to a function object. You can even assign a function to a new name:

```python
def greet():
    return "Hello!"
```

## Calling Functions
To execute a function, write its name followed by parentheses.
```python
greet()         # calls the function defined above
```

>[!IMPORTANT]
>A function must be **defined before it is called** (or be in scope).

## Function Parameters

Parameters allow functions to be **reusable**. Instead of writing a function that only knows how to handle "Carrots," you use a parameter so it can handle any `seed_type` you provide.

What's the difference between parameters and arguments?
* **Parameters:** The variable names listed in the function definition (the "labels").
* **Arguments:** The actual values you pass into the function when you call it (the "data").

| Type | Syntax Example | Description |
| :--- | :--- | :--- |
| Positional | `def func(a, b):` | Values must be passed in the exact order defined. |
| Keyword | `func(b=2, a=1)` | You specify the name, so the order doesn't matter. |
| Default | `def func(a=10)` | If no argument is provided, it uses the value 10. |
| Type-Hinted | `def func(a: int)` | Tells developers/tools what data type is expected. |

How does data flow?
1. Input: You pass an argument into the function call.
2. Assignment: The parameter variable is "assigned" that value internally.
3. Execution: The function uses that variable to perform its logic.
4. Scope: Once the function finishes, the parameter variables are deleted from memory.

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
