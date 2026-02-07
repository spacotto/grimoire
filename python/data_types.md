# About Python Data Types
Python has several built-in data types for storing different kinds of information.

>[!TIP]
>Check the type of any object by using `type()` or `isinstance()`:
>```python
>type(42)        # <class 'int'>
>type("hello")   # <class 'str'>
>isinstance(42, int)  # True
>```

>[!NOTE]
Data can be converted:
>```python
>int("42")       # 42
>float(10)       # 10.0
>str(3.14)       # "3.14"
>list("abc")     # ['a', 'b', 'c']
>tuple([1, 2])   # (1, 2)
>set([1, 1, 2])  # {1, 2}
>```

## Numeric Types
### int
Whole numbers without decimal points.
```python
x = 42
y = -17
z = 0
```

### float
Numbers with decimal points.
```python
pi = 3.14159
temp = -40.5
```

### complex
Numbers with real and imaginary parts.
```python
c = 3 + 4j
```

## Sequence Types
### str
Text data enclosed in quotes.
```python
name = "Alice"
message = 'Hello, World!'
multiline = """This is
a multiline string"""
```

### list
Ordered, mutable collection of items.
```python
numbers = [1, 2, 3, 4, 5]
mixed = [1, "two", 3.0, True]
nested = [[1, 2], [3, 4]]
```

### tuple
Ordered, immutable collection of items.
```python
coordinates = (10, 20)
single = (1,)  # Note the comma
```

### range
Immutable sequence of numbers.
```python
r = range(5)        # 0, 1, 2, 3, 4
r = range(1, 10)    # 1 through 9
r = range(0, 10, 2) # 0, 2, 4, 6, 8
```

## Mapping Type
### dict
Key-value pairs.
```python
person = {"name": "Bob", "age": 30}
grades = dict(math=95, english=88)
```

## Set Types
### set
Unordered collection of unique items.
```python
unique = {1, 2, 3, 3, 4}  # Results in {1, 2, 3, 4}
```

### frozenset
Immutable version of set.
```python
fs = frozenset([1, 2, 3])
```

## Boolean Type
### bool
Truth values: `True` or `False`.
```python
is_valid = True
is_empty = False
```

## None Type
### NoneType
Represents absence of a value.
```python
result = None
```
