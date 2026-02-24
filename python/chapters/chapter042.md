# Sets
Sets are unordered collections of unique elements in Python. They provide fast membership testing and efficient operations for mathematical set theory, making them ideal for removing duplicates, testing membership, and performing operations like unions and intersections.

## What are Sets?

A set is a built-in Python data type that **stores an unordered collection of unique items**. Sets are **mutable** (you can add/remove elements), but the **elements** themselves must be **immutable** (**hashable**) types like strings, numbers, or tuples.

Key characteristics:
- Unordered (no indexing)
- Unique elements only (no duplicates)
- Mutable (can be modified)
- Fast membership testing

## Creating Sets

Use curly braces `{}` or the `set()` constructor:

```python
# Using curly braces
fruits = {'apple', 'banana', 'orange'}

# Using set() constructor
numbers = set([1, 2, 3, 4])

# Empty set (must use set(), not {})
empty = set()  # Correct
not_empty = {}  # This creates an empty dictionary!

# From a string (creates set of characters)
letters = set('hello')  # {'h', 'e', 'l', 'o'}
```

## Set Uniqueness Property

Sets automatically remove duplicates:

```python
numbers = {1, 2, 2, 3, 3, 3}
print(numbers)  # {1, 2, 3}

# Remove duplicates from a list
my_list = [1, 2, 2, 3, 4, 4, 5]
unique = set(my_list)  # {1, 2, 3, 4, 5}
```

## Adding and Removing Elements

```python
colors = {'red', 'blue'}

# Add single element
colors.add('green')

# Add multiple elements
colors.update(['yellow', 'purple'])

# Remove element (raises KeyError if not found)
colors.remove('red')

# Remove element (no error if not found)
colors.discard('orange')

# Remove and return arbitrary element
color = colors.pop()

# Clear all elements
colors.clear()
```

## Set Operations

### Union (|)

Combines all elements from both sets:

```python
set1 = {1, 2, 3}
set2 = {3, 4, 5}

result = set1 | set2  # {1, 2, 3, 4, 5}
```

### Intersection (&)

Returns only common elements:

```python
set1 = {1, 2, 3}
set2 = {2, 3, 4}

result = set1 & set2  # {2, 3}
```

### Difference (-)

Returns elements in first set but not in second:

```python
set1 = {1, 2, 3}
set2 = {2, 3, 4}

result = set1 - set2  # {1}
```

### Symmetric Difference (^)

Returns elements in either set, but not in both:

```python
set1 = {1, 2, 3}
set2 = {2, 3, 4}

result = set1 ^ set2  # {1, 4}
```

## Set Methods

Alternative syntax using methods:

```python
set1 = {1, 2, 3}
set2 = {3, 4, 5}

# Union
set1.union(set2)  # {1, 2, 3, 4, 5}

# Intersection
set1.intersection(set2)  # {3}

# Difference
set1.difference(set2)  # {1, 2}

# Symmetric difference
set1.symmetric_difference(set2)  # {1, 2, 4, 5}
```

## Subset and Superset Operations

```python
set1 = {1, 2}
set2 = {1, 2, 3, 4}

# Check if subset
set1.issubset(set2)  # True
set1 <= set2  # True

# Check if superset
set2.issuperset(set1)  # True
set2 >= set1  # True

# Check if disjoint (no common elements)
set1.isdisjoint({5, 6})  # True
```

## Set Comprehensions (Preview)

Create sets using comprehension syntax:

```python
# Squares of even numbers
squares = {x**2 for x in range(10) if x % 2 == 0}
# {0, 4, 16, 36, 64}

# Unique characters from a string
chars = {c.lower() for c in "Hello World" if c.isalpha()}
# {'h', 'e', 'l', 'o', 'w', 'r', 'd'}
```

## When to Use Sets

Use sets when you need:
- **Fast membership testing**: `if item in my_set` is O(1)
- **Duplicate removal**: Automatically enforced uniqueness
- **Mathematical operations**: Union, intersection, difference
- **Unordered collections**: Order doesn't matter

Don't use sets when:
- You need to maintain order (use list)
- You need indexing (use list)
- Elements must be mutable (use list)
- You need duplicate values (use list)

## Practical Set Applications

```python
# Remove duplicates
emails = ['a@ex.com', 'b@ex.com', 'a@ex.com']
unique_emails = set(emails)

# Find common interests
alice_hobbies = {'reading', 'hiking', 'coding'}
bob_hobbies = {'gaming', 'hiking', 'cooking'}
common = alice_hobbies & bob_hobbies  # {'hiking'}

# Find unique visitors
day1_visitors = {'user1', 'user2', 'user3'}
day2_visitors = {'user2', 'user4', 'user5'}
new_visitors = day2_visitors - day1_visitors  # {'user4', 'user5'}

# Check permissions
user_permissions = {'read', 'write'}
required_permissions = {'read', 'write', 'execute'}
has_all = user_permissions >= required_permissions  # False

# Filter valid items
valid_items = {'apple', 'banana', 'orange'}
user_input = ['apple', 'grape', 'banana', 'kiwi']
valid_only = [item for item in user_input if item in valid_items]
# ['apple', 'banana']
```
