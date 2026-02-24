# Tuples
Tuples are immutable sequences in Python used to store collections of items. They're similar to lists but cannot be modified after creation, making them ideal for fixed collections and function return values.

## What are Tuples?

A tuple is an **ordered, immutable collection of elements**. Once created, you cannot add, remove, or change elements. Tuples can contain **any data type and allow duplicates**.

## Creating Tuples

```python
# Empty tuple
empty = ()

# Single element (note the comma)
single = (5,)

# Multiple elements
coordinates = (10, 20)
mixed = (1, "hello", 3.14, True)

# Without parentheses (tuple packing)
point = 3, 4
```

## Tuple Immutability

Tuples cannot be modified after creation:

```python
my_tuple = (1, 2, 3)
# my_tuple[0] = 10  # TypeError: 'tuple' object does not support item assignment
```

However, if a tuple contains mutable objects, those objects can be modified:

```python
tuple_with_list = (1, [2, 3], 4)
tuple_with_list[1].append(5)  # Valid: (1, [2, 3, 5], 4)
```

## Accessing Tuple Elements

Use indexing and slicing like lists:

```python
colors = ("red", "green", "blue", "yellow")

print(colors[0])      # "red"
print(colors[-1])     # "yellow"
print(colors[1:3])    # ("green", "blue")
```

## Tuple Unpacking

Extract tuple values into separate variables:

```python
point = (5, 10)
x, y = point  # x = 5, y = 10

# Ignore values with underscore
name, _, age = ("Alice", "Smith", 30)  # name = "Alice", age = 30

# Extended unpacking with *
first, *middle, last = (1, 2, 3, 4, 5)  # first = 1, middle = [2, 3, 4], last = 5
```

## Multiple Assignment with Tuples

Assign multiple variables in one line:

```python
a, b, c = 1, 2, 3

# Swap values without temporary variable
x, y = 10, 20
x, y = y, x  # x = 20, y = 10
```

## Tuples as Return Values

Functions can return multiple values as a tuple:

```python
def get_stats(numbers):
    return min(numbers), max(numbers), sum(numbers)

minimum, maximum, total = get_stats([1, 2, 3, 4, 5])
```

## Common Tuple Operations

```python
t = (1, 2, 3, 2, 4)

# Length
len(t)  # 5

# Count occurrences
t.count(2)  # 2

# Find index
t.index(3)  # 2

# Membership
2 in t  # True

# Concatenation
(1, 2) + (3, 4)  # (1, 2, 3, 4)

# Repetition
(1, 2) * 3  # (1, 2, 1, 2, 1, 2)

# Iteration
for item in t:
    print(item)
```

## Named Tuples

Create tuples with named fields for better readability:

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)

print(p.x)  # 10
print(p.y)  # 20
print(p[0])  # 10 (still works like regular tuple)
```

## When to Use Tuples

Use tuples when:

- Data should not change (coordinates, RGB values, database records)
- Returning multiple values from functions
- Using as dictionary keys (lists cannot be keys)
- Unpacking values for clarity
- Optimizing for performance (tuples are slightly faster and use less memory)

## Tuples vs. Lists

| Feature | Tuple | List |
|---------|-------|------|
| Mutability | Immutable | Mutable |
| Syntax | `()` | `[]` |
| Performance | Faster | Slower |
| Memory | Less | More |
| Use case | Fixed data | Dynamic data |
| Dictionary key | Yes | No |

```python
# Tuple: fixed collection
point = (3, 4)

# List: dynamic collection
scores = [85, 92, 78]
scores.append(95)  # Can modify
```
