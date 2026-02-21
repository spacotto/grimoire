# About `range()` Function
`range()` generates a sequence of numbers, commonly used in `for` loops.

## Syntax
```python
range(stop)
range(start, stop)
range(start, stop, step)
```

## Parameters
- **stop**: End value (exclusive)
- **start**: Starting value (inclusive, default: 0)
- **step**: Increment between values (default: 1)

## Examples
### Basic usage
```python
for i in range(5):
    print(i)
# Output: 0, 1, 2, 3, 4
```

### With start and stop
```python
for i in range(2, 6):
    print(i)
# Output: 2, 3, 4, 5
```

### With step
```python
for i in range(0, 10, 2):
    print(i)
# Output: 0, 2, 4, 6, 8
```

### Negative step (counting down)
```python
for i in range(10, 0, -1):
    print(i)
# Output: 10, 9, 8, 7, 6, 5, 4, 3, 2, 1
```

## Key Points
- `range()` returns a range object (not a list)
- Stop value is **never included** in the output
- Use `list(range(5))` to convert to a list: `[0, 1, 2, 3, 4]`
- All parameters must be integers
- Step cannot be zero (raises `ValueError`)

## Common Use Cases
- Iterating a specific number of times
- Generating number sequences
- Indexing through lists/strings
