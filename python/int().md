# About `int()`
`int` is a built-in type (class) in Python. When you call `int()`, you're actually calling the class constructor, which creates an integer object. In other words, `int()` converts a value to an integer.

Example of syntax:
```python
int(x=0)
int(x, base=10)
```

## Parameters
- `x` (optional): A number or string to convert. Defaults to 0.
- `base` (optional): The number base for string conversion (2-36). Defaults to 10. Only valid when x is a string.

## Return Value
Returns an integer object.

## Usage
### Convert from float
It truncates toward zero (drops decimal portion):
```python
int(3.14)      # 3
int(-7.9)      # -7
int(5.0)       # 5
```

### Convert from string
```python
int("42")      # 42
int("-100")    # -100
int("   50  ") # 50 (whitespace ignored)
```

### Convert with different bases
```python
int("1010", 2)    # 10 (binary)
int("ff", 16)     # 255 (hexadecimal)
int("77", 8)      # 63 (octal)
int("z", 36)      # 35 (base-36)
```

Prefix notation works with base inference:
```python
int("0b1010", 0)  # 10 (auto-detects binary)
int("0xff", 0)    # 255 (auto-detects hex)
int("0o77", 0)    # 63 (auto-detects octal)
```

### Default value
```python
int()  # 0
```

### Common Errors
```python
int("abc")        # ValueError: invalid literal
int(3.14, 2)      # TypeError: can't convert with base
int("10", 37)     # ValueError: base must be 2-36
int(None)         # TypeError: int() argument must be a string or number
```

## Notes
- Always truncates toward zero, never rounds
- When converting from float, fractional part is discarded
- Leading/trailing whitespace in strings is ignored
- The `base` parameter only works with string inputs
- For boolean values: `int(True)` → `1`, `int(False)` → `0`
