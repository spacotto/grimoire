# About Function Definitions
A function is a **block of reusable code that performs a specific task**. It's a way to package code so you can run it multiple times without rewriting it.

>[!TIP]
>Think of a function as a recipe: you define it once with instructions, then you can use it whenever you need it by calling its name.

## Structure
```python
def function_name(parameters):
    # function body
```

### Code Breakdown
| Code            | Description                               |
| :-------------- | :---------------------------------------- |
| `def`           | Keyword that starts a function definition |
| `function_name` | The name you give your function           |
| `parameters`    | Inputs the function accepts (optional)    |
| `:`             | Indicates the start of the function body  |

>[!IMPORTANT]
>The **indentation** (4 spaces, NOT `TAB`!) defines the **function's scope**.

### Example
```python
# Define the function
def greet(name):
    return f"Hello, {name}!"

# Use the function
message = greet("World")
print(message)  # Output: Hello, World!
```

>[!TIP]
>The function name should be descriptive and follow `snake_case` convention.

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
