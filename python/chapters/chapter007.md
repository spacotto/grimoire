# Arithmetic Operations
Python 3 supports standard arithmetic operations out of the box. These operations work on integers (`int`) and floating-point numbers (`float`) and form the foundation of any calculation in Python.

## Basic Arithmetic Operators

| Operator | Operation        | Example   | Result |
|----------|------------------|-----------|--------|
| `+`      | Addition         | `3 + 2`   | `5`    |
| `-`      | Subtraction      | `3 - 2`   | `1`    |
| `*`      | Multiplication   | `3 * 2`   | `6`    |
| `/`      | Division         | `3 / 2`   | `1.5`  |
| `//`     | Floor Division   | `3 // 2`  | `1`    |
| `%`      | Modulus          | `3 % 2`   | `1`    |
| `**`     | Exponentiation   | `3 ** 2`  | `9`    |

## Addition and Subtraction

```python
print(10 + 5)   # 15
print(10 - 5)   # 5
print(1.5 + 2)  # 3.5 — mixing int and float returns float
```

>[!NOTE]
>Mixing `int` and `float` always returns a `float`.

## Multiplication and Division

```python
print(4 * 3)    # 12
print(10 / 4)   # 2.5  — always returns float
print(10 // 4)  # 2    — floor division, returns int
print(10 % 3)   # 1    — remainder
print(2 ** 8)   # 256  — exponentiation
```

>[!NOTE]
>`/` always returns a `float`, even if the result is whole (e.g., `4 / 2 → 2.0`).

## Performing Calculations with Variables

```python
a = 12
b = 4

print(a + b)   # 16
print(a - b)   # 8
print(a * b)   # 48
print(a / b)   # 3.0
print(a ** 2)  # 144
```

>[!NOTE]
>Variables hold values that can be used directly in any arithmetic expression.

## Storing Calculation Results

```python
x = 10
y = 3

total      = x + y   # 13
difference = x - y   # 7
product    = x * y   # 30
quotient   = x / y   # 3.333...
remainder  = x % y   # 1
power      = x ** y  # 1000

print(total, difference, product, quotient, remainder, power)
```

>[!NOTE]
>Results can be stored in new variables for reuse, chaining, or further computation.
