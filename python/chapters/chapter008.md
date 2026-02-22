# Comparison Operators
Comparison operators evaluate the relationship between two values and always return a **boolean** result: `True` or `False`. They are fundamental to control flow, conditions, and logic in Python.

## Greater Than (`>`)

Returns `True` if the left operand is strictly greater than the right.

```python
5 > 3   # True
3 > 5   # False
5 > 5   # False
```


## Less Than (`<`)

Returns `True` if the left operand is strictly less than the right.

```python
3 < 5   # True
5 < 3   # False
5 < 5   # False
```

## Equal To (`==`)

Returns `True` if both operands have the same value. Note: `==` checks value equality, not identity (use `is` for identity).

```python
5 == 5      # True
5 == 3      # False
"a" == "a"  # True
```

## Not Equal To (`!=`)

Returns `True` if both operands do **not** have the same value.

```python
5 != 3      # True
5 != 5      # False
"a" != "b"  # True
```

## Greater Than or Equal (`>=`)

Returns `True` if the left operand is greater than **or equal to** the right.

```python
5 >= 5   # True
6 >= 5   # True
4 >= 5   # False
```

## Less Than or Equal (`<=`)

Returns `True` if the left operand is less than **or equal to** the right.

```python
5 <= 5   # True
4 <= 5   # True
6 <= 5   # False
```

## Boolean Results

All comparison operators return a `bool` — either `True` or `False`. These results can be:

- Stored in a variable: `result = 10 > 5  # True`
- Used in conditionals: `if x >= 0:`
- Combined with logical operators (`and`, `or`, `not`):

```python
x = 7
print(x > 0 and x < 10)   # True
print(x < 0 or x == 7)    # True
print(not x == 7)          # False
```

>[TIP]
>Comparisons can be chained in Python: `0 < x < 10` is valid and equivalent to `x > 0 and x < 10`.
