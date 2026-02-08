# About Functions
A function is a **block of reusable code that performs a specific task**. It's a way to package code so you can run it multiple times without rewriting it.

>[!TIP]
>Think of a function as a recipe: you define it once with instructions, then you can use it whenever you need it by calling its name.

Basic structure:
```python
def function_name(parameters):
    # function body
    return #optional
```
### Anatomy of a Function
A function consists of several elements, some mandatory (they must always be present), some optional.
1. **The header.** The **header** is the first **line that defines the function’s interface**. It includes several elements, some mandatory (they must always be present), some optional.
2. **The docstring**
3. **The scope**

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

## The Function Name
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

# Assigning the function object to a new variable
say_hi = greet 

print(say_hi())  # Output: Hello!
```

## The Parameters
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

## The Function Signatures
In Python, specifying a return type (e.g., `-> None`) is part of **Type Hinting**. While the Python interpreter doesn't require it to run the code, it is a standard practice for maintainable software.

Why use `-> None`?
* **Communication:** It explicitly tells other developers that the function is a **side-effect** function (it performs an action like printing or saving a file) and does not produce a data result.
* **Static Analysis:** Tools like `Mypy` or IDEs (VS Code, PyCharm) use this to ensure you don't accidentally try to assign the result of a "void" function to a variable.
* **Consistency:** If your codebase uses type hints for parameters (`seed_type: str`), it is best practice to provide the return type for a complete signature.

| Type | Definition | Usage Intent |
| :--- | :--- | :--- |
| **Value-Returning** | `def add(a: int) -> int:` | Returns data to be used in further logic. |
| **Side-Effect Only** | `def log(a: int) -> None:` | Performs an action; returns nothing useful. |

## Docstring

## Scope
>[!IMPORTANT]
>The **indentation** (4 spaces, NOT `TAB`!) defines the **function's scope**.



