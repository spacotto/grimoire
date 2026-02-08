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
# Anatomy of a Function
A function consists of several elements, some mandatory (they must always be present), some optional.
1. The header
2. The Scope

## The Header
The **header** is the first **line that defines the function’s interface**. It includes several elements, some mandatory (they must always be present), some optional.

| Element  | Syntax   | Function | Presence |
| :------- | :------- | :------- | :------- | 
| Defining keyword | `def` | Signals the start of the definition | Mandatory |
| Function name | `snake_case` | A unique identifier | Mandatory |
| Parameters | `(snake_case)` | Variables listed inside parentheses `()` that receive input values (arguments) | Optional |
| Return type | `-> ...` | The exptected data to return | Optional |
| Colon | `:` | Terminates the header and initiates the code block | Mandatory |


## `def`
In Python, `def` is a **keyword** used to define a function. It **marks the start** of a function header and signals to the interpreter that a reusable block of code is being created.

>[!TIP]
>The function name should be descriptive and follow `snake_case` convention.

## Parameters
Parameters allow functions to be **reusable**. Instead of writing a function that only knows how to handle "Carrots," you use a parameter so it can handle any `seed_type` you provide.

### 1. Parameters vs. Arguments
* **Parameters:** The variable names listed in the function definition (the "labels").
* **Arguments:** The actual values you pass into the function when you call it (the "data").

### 2. Anatomy of a Parameterized Function
```python
# 'name' and 'count' are parameters
def greet_gardener(name: str, count: int) -> None:
    print(f"Hello {name}, you have {count} seeds.")

# "Alice" and 50 are arguments
greet_gardener("Alice", 50)
```

### 3. Common Parameter Types
| Type | Syntax Example | Description |
| :--- | :--- | :--- |
| Positional | `def func(a, b):` | Values must be passed in the exact order defined. |
| Keyword | `func(b=2, a=1)` | You specify the name, so the order doesn't matter. |
| Default | `def func(a=10)` | If no argument is provided, it uses the value 10. |
| Type-Hinted | `def func(a: int)` | Tells developers/tools what data type is expected. |

### 4. How Data Flows
1. Input: You pass an argument into the function call.
2. Assignment: The parameter variable is "assigned" that value internally.
3. Execution: The function uses that variable to perform its logic.
4. Scope: Once the function finishes, the parameter variables are deleted from memory.

## Function Signatures: Explicit Return Hints
In Python, specifying a return type (e.g., `-> None`) is part of **Type Hinting**. While the Python interpreter doesn't require it to run the code, it is a standard practice for maintainable software.

### Why use `-> None`?
* **Communication:** It explicitly tells other developers that the function is a **side-effect** function (it performs an action like printing or saving a file) and does not produce a data result.
* **Static Analysis:** Tools like `Mypy` or IDEs (VS Code, PyCharm) use this to ensure you don't accidentally try to assign the result of a "void" function to a variable.
* **Consistency:** If your codebase uses type hints for parameters (`seed_type: str`), it is best practice to provide the return type for a complete signature.

### Example Comparison
| Type | Definition | Usage Intent |
| :--- | :--- | :--- |
| **Value-Returning** | `def add(a: int) -> int:` | Returns data to be used in further logic. |
| **Side-Effect Only** | `def log(a: int) -> None:` | Performs an action; returns nothing useful. |

## Scope
>[!IMPORTANT]
>The **indentation** (4 spaces, NOT `TAB`!) defines the **function's scope**.

