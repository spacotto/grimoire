# Dictionaries
Dictionaries are one of Python's most powerful built-in data structures. They store data as key-value pairs, allowing fast lookups and flexible data organisation. This guide covers dictionary fundamentals, common operations, and practical use cases.

## What are Dictionaries?

A dictionary is a mutable, unordered collection that stores data as key-value pairs. Each key maps to a specific value, similar to a real-world dictionary where words (keys) map to definitions (values).

**Key characteristics:**
- Keys must be unique and immutable (strings, numbers, tuples)
- Values can be any data type
- Fast lookups by key (O(1) average time complexity)
- Unordered (prior to Python 3.7) or insertion-ordered (Python 3.7+)

## Creating Dictionaries

```python
# Empty dictionary
empty_dict = {}
empty_dict = dict()

# Dictionary with initial data
student = {"name": "Alice", "age": 20, "major": "CS"}

# Using dict() constructor
scores = dict(math=95, english=87, history=92)

# From list of tuples
items = dict([("a", 1), ("b", 2), ("c", 3)])
```

## Key-Value Pairs

Each entry in a dictionary consists of a key and its associated value, separated by a colon.
```python
person = {
    "name": "Bob",        # key: "name", value: "Bob"
    "age": 25,            # key: "age", value: 25
    "city": "New York"    # key: "city", value: "New York"
}
```

## Accessing Values

Access values using square bracket notation or the `get()` method.
```python
student = {"name": "Alice", "grade": "A"}

# Using square brackets
print(student["name"])  # Output: Alice

# Using get() - safer, returns None if key doesn't exist
print(student.get("grade"))  # Output: A
print(student.get("age"))    # Output: None
```

## Adding and Updating Items

```python
student = {"name": "Alice", "age": 20}

# Add new key-value pair
student["major"] = "Biology"

# Update existing value
student["age"] = 21

# Result: {"name": "Alice", "age": 21, "major": "Biology"}
```

## Removing Items

### `del` - Remove specific key
```python
student = {"name": "Alice", "age": 20, "major": "CS"}
del student["age"]
# Result: {"name": "Alice", "major": "CS"}
```

### `pop()` - Remove and return value
```python
major = student.pop("major")
print(major)  # Output: CS

# Provide default if key doesn't exist
gpa = student.pop("gpa", 0.0)
```

### `popitem()` - Remove and return last inserted pair
```python
student = {"name": "Alice", "age": 20}
item = student.popitem()
print(item)  # Output: ("age", 20)
```

### `clear()` - Remove all items
```python
student.clear()
# Result: {}
```

## Dictionary Methods

### `keys()` - Get all keys
```python
student = {"name": "Alice", "age": 20, "major": "CS"}
keys = student.keys()
print(list(keys))  # Output: ["name", "age", "major"]
```

### `values()` - Get all values
```python
values = student.values()
print(list(values))  # Output: ["Alice", 20, "CS"]
```

### `items()` - Get all key-value pairs
```python
items = student.items()
print(list(items))  # Output: [("name", "Alice"), ("age", 20), ("major", "CS")]
```

### `get()` - Retrieve value with default
```python
age = student.get("age", 18)        # Returns 20
gpa = student.get("gpa", 0.0)       # Returns 0.0 (default)
```

### `update()` - Merge dictionaries
```python
student = {"name": "Alice", "age": 20}
student.update({"age": 21, "major": "CS"})
# Result: {"name": "Alice", "age": 21, "major": "CS"}
```

## Checking for Key Existence

```python
student = {"name": "Alice", "age": 20}

# Using 'in' keyword
if "name" in student:
    print("Name exists")

if "gpa" not in student:
    print("GPA not found")
```

## Iterating Over Dictionaries

### Iterate over keys
```python
for key in student:
    print(key)
```

### Iterate over values
```python
for value in student.values():
    print(value)
```

### Iterate over key-value pairs
```python
for key, value in student.items():
    print(f"{key}: {value}")
```

## Nested Dictionaries

Dictionaries can contain other dictionaries as values.
```python
students = {
    "student1": {"name": "Alice", "age": 20, "major": "CS"},
    "student2": {"name": "Bob", "age": 22, "major": "Math"}
}

# Accessing nested values
print(students["student1"]["name"])  # Output: Alice

# Iterating over nested dictionaries
for student_id, info in students.items():
    print(f"{student_id}: {info['name']}")
```

## Dictionary Comprehensions (Preview)

Dictionary comprehensions provide a concise way to create dictionaries.
```python
# Create dictionary from range
squares = {x: x**2 for x in range(5)}
# Result: {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Filter with condition
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}
```

## When to Use Dictionaries

Use dictionaries when you need:
- Fast lookups by a unique identifier
- To associate related data (key-value relationships)
- To count occurrences of items
- To group or categorize data
- Configuration settings or options
- Caching computed results

**Common use cases:**
- User profiles (username → user data)
- Word frequency counts
- Database-like records
- JSON data representation
- Function keyword arguments
