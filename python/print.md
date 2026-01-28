# About `print()`
The `print()` function outputs text and other data to the console (standard output).

**Basic Syntax:**
```python
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```

## Parameters
| Parameter  | Description                                        | Default      |
| :--------- | :------------------------------------------------- | :----------- |
| `*objects` | One or more values to print (converted to strings) | -            |
| `sep`      | Separator between multiple values                  | space        |
| `end`      | String appended after output                       | newline      |
| `file`     | File object to write to                            | `sys.stdout` |
| `flush`    | Force flush the output buffer                      | `False`      |

>[!NOTE]
>All non-string objects are converted using `str()`.

>[!TIP]
>Use `repr()` or f-strings for more control over formatting

## Examples
### Basic Usage
```python
print("Hello, World!")
# Output: Hello, World!

print(42)
# Output: 42
```

### Multiple Values
```python
print("Name:", "Alice", "Age:", 30)
# Output: Name: Alice Age: 30
```

### Custom Separator
```python
print("apple", "banana", "cherry", sep=", ")
# Output: apple, banana, cherry
```

### Custom End Character
```python
print("Loading", end="... ")
print("Done!")
# Output: Loading... Done!
```

### Printing to a File
```python
with open("output.txt", "w") as f:
    print("Hello, file!", file=f)
```

## `print("...")` vs. `print(f"...")`
When using `print()`, you can choose between plain strings (`"..."`) and f-strings (`f"..."`). Plain strings print **exactly what's written**, while **f-strings allow you to embed variables and expressions** inside curly braces `{}`. For example, `print("Total: cost")` outputs literally "Total: cost", but `print(f"Total: {cost}")` evaluates the `cost` variable and inserts its value. F-strings also support expressions like `{2 + 2}`, method calls like `{name.upper()}`, and formatting options like `{pi:.2f}` for two decimal places. Use plain strings for static text and f-strings when you need dynamic content.

## Comparison Table

| Feature | Plain String `"..."` | f-string `f"..."` |
|---------|---------------------|-------------------|
| Variable insertion | ❌ Not supported | ✅ `{variable}` |
| Expression evaluation | ❌ Not supported | ✅ `{2 + 2}` |
| Method calls | ❌ Not supported | ✅ `{name.upper()}` |
| Number formatting | ❌ Not supported | ✅ `{pi:.2f}` |
| Performance | Slightly faster | Slightly slower (negligible) |
| Use case | Static text | Dynamic content |

### Examples Side-by-Side
```python
name = "Alice"
age = 30

# Plain string
print("Name: name, Age: age")
# Output: Name: name, Age: age

# f-string
print(f"Name: {name}, Age: {age}")
# Output: Name: Alice, Age: 30
```
