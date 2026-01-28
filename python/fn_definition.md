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

>[TIP]
>The function name should be descriptive and follow `snake_case` convention.

