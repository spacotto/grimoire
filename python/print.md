# About `print()` Function Documentation
The `print()` function outputs text and other data to the console (standard output).

## Basic Syntax
```python
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```

## Parameters

- **`*objects`** — One or more values to print (converted to strings)
- **`sep`** — Separator between multiple values (default: space)
- **`end`** — String appended after output (default: newline)
- **`file`** — File object to write to (default: `sys.stdout`)
- **`flush`** — Force flush the output buffer (default: `False`)

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

## Common Use Cases

- Debugging and logging
- Displaying program output
- Progress indicators
- Writing to files

## Notes

- All non-string objects are converted using `str()`
- Use `repr()` or f-strings for more control over formatting
- For complex formatting, consider f-strings or `.format()`
