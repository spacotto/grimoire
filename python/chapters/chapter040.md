# Lists
Lists are Python's most versatile and commonly used data structure for storing ordered collections of items. They're mutable, meaning you can change their contents after creation, and can hold any mix of data types. This guide covers essential list operations and methods you'll use daily.

## What are Lists?

A list is an **ordered, mutable collection that can contain any type of data**. Lists **maintain insertion order** and **allow duplicate values**.

```python
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]
nested = [[1, 2], [3, 4]]
empty = []
```

## Creating Lists

```python
# Direct creation
fruits = ["apple", "banana", "cherry"]

# Using list() constructor
digits = list(range(5))  # [0, 1, 2, 3, 4]

# From string
chars = list("abc")  # ['a', 'b', 'c']

# Empty list
items = []
# or
items = list()
```

## List Indexing and Slicing

```python
fruits = ["apple", "banana", "cherry", "date"]

# Indexing (0-based)
fruits[0]   # "apple"
fruits[-1]  # "date" (last item)

# Slicing [start:stop:step]
fruits[1:3]   # ["banana", "cherry"]
fruits[:2]    # ["apple", "banana"]
fruits[2:]    # ["cherry", "date"]
fruits[::2]   # ["apple", "cherry"] (every 2nd item)
fruits[::-1]  # ["date", "cherry", "banana", "apple"] (reversed)
```

## Adding Elements

```python
numbers = [1, 2, 3]

# append() - add single item to end
numbers.append(4)        # [1, 2, 3, 4]

# insert() - add item at specific position
numbers.insert(1, 1.5)   # [1, 1.5, 2, 3, 4]

# extend() - add multiple items
numbers.extend([5, 6])   # [1, 1.5, 2, 3, 4, 5, 6]

# += operator - shorthand for extend()
numbers += [7, 8]        # [1, 1.5, 2, 3, 4, 5, 6, 7, 8]
```

## Removing Elements

```python
fruits = ["apple", "banana", "cherry", "banana"]

# remove() - remove first occurrence of value
fruits.remove("banana")  # ["apple", "cherry", "banana"]

# pop() - remove and return item by index (default: -1)
last = fruits.pop()    # returns "banana"
first = fruits.pop(0)  # returns "apple"

# clear() - remove all items
fruits.clear()  # []
```

## List Operations

```python
# Concatenation
[1, 2] + [3, 4]  # [1, 2, 3, 4]

# Repetition
[0] * 5  # [0, 0, 0, 0, 0]

# Membership
3 in [1, 2, 3]  # True
```

## Common List Methods

```python
items = [3, 1, 4, 1, 5]

items.count(1)       # 2 (occurrences of 1)
items.index(4)       # 2 (position of first 4)
items.reverse()      # [5, 1, 4, 1, 3] (in-place)
items.copy()         # creates shallow copy
```

## List Iteration

```python
fruits = ["apple", "banana", "cherry"]

# Basic loop
for fruit in fruits:
    print(fruit)

# With index
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")

# While loop
i = 0
while i < len(fruits):
    print(fruits[i])
    i += 1
```

## Built-in Functions for Lists

```python
numbers = [3, 1, 4, 1, 5]

len(numbers)    # 5
sum(numbers)    # 14
max(numbers)    # 5
min(numbers)    # 1
any(numbers)    # True (if any element is truthy)
all(numbers)    # True (if all elements are truthy)
```

## Sorting Lists

```python
numbers = [3, 1, 4, 1, 5]

# sort() - sorts in-place, returns None
numbers.sort()           # numbers becomes [1, 1, 3, 4, 5]
numbers.sort(reverse=True)  # [5, 4, 3, 1, 1]

# sorted() - returns new sorted list
original = [3, 1, 4]
new_list = sorted(original)  # [1, 3, 4], original unchanged

# Custom sorting
words = ["banana", "apple", "cherry"]
words.sort(key=len)  # sort by length
```

## List Comprehensions (Preview)

A concise way to create lists based on existing sequences:
```python
# Traditional approach
squares = []
for x in range(5):
    squares.append(x ** 2)

# List comprehension
squares = [x ** 2 for x in range(5)]  # [0, 1, 4, 9, 16]

# With condition
evens = [x for x in range(10) if x % 2 == 0]  # [0, 2, 4, 6, 8]
```

## When to Use Lists

Use lists when you need:

- An ordered collection that can change
- To store multiple items of any type
- Fast access by index
- To append items frequently

Consider alternatives when:

- You need unique items only → use `set`
- You need key-value pairs → use `dict`
- Data shouldn't change → use `tuple`
- You need high-performance numeric operations → use `numpy.array`
