# Working with Collections
Collections are fundamental data structures in Python for storing and organizing multiple values. This guide covers Python's built-in collection types (lists, tuples, sets, dictionaries), their performance characteristics, and practical patterns for working with them effectively. Understanding when and how to use each collection type is essential for writing efficient, readable Python code.

## Choosing the Right Data Structure

**List** - Use when you need:
- Ordered sequence of items
- Duplicate values allowed
- Items accessed by index
- Dynamic resizing (append/remove frequently)

**Tuple** - Use when you need:
- Immutable ordered sequence
- Fixed collection of related values
- Dictionary keys or set members
- Slightly better performance than lists for read operations

**Set** - Use when you need:
- Unique values only
- Fast membership testing
- Mathematical set operations (union, intersection, difference)
- Order doesn't matter

**Dictionary** - Use when you need:
- Key-value pairs
- Fast lookups by key
- Associative relationships
- Structured data with named fields

## Collection Performance Characteristics

### Time Complexity Summary

**List Operations:**
- Access by index: O(1)
- Append to end: O(1)
- Insert at position: O(n)
- Search for value: O(n)
- Delete by index: O(n)

**Set Operations:**
- Add element: O(1)
- Remove element: O(1)
- Membership test: O(1)
- Union/intersection: O(len(s) + len(t))

**Dictionary Operations:**
- Get value by key: O(1)
- Set key-value: O(1)
- Delete key: O(1)
- Check if key exists: O(1)

### Memory Considerations

Sets and dictionaries use more memory than lists due to hash tables. Tuples use slightly less memory than lists. Choose based on your access patterns rather than memory alone unless working with very large datasets.

## Common Collection Patterns

### List Patterns
```python
# List comprehension for transformation
squares = [x**2 for x in range(10)]

# Filtering with comprehension
evens = [x for x in numbers if x % 2 == 0]

# Slicing
first_three = items[:3]
last_two = items[-2:]
reversed_list = items[::-1]

# Unpacking
first, *rest = items
*beginning, last = items

# Enumerate for index + value
for i, value in enumerate(items):
    print(f"{i}: {value}")
```

### Dictionary Patterns
```python
# Dictionary comprehension
squared_dict = {x: x**2 for x in range(5)}

# Default values
count = counts.get(key, 0)
# or
from collections import defaultdict
counts = defaultdict(int)

# Merging dictionaries
combined = {**dict1, **dict2}  # Python 3.5+
combined = dict1 | dict2  # Python 3.9+

# Inverting key-value pairs
inverted = {v: k for k, v in original.items()}
```

### Set Patterns
```python
# Remove duplicates from list
unique_items = list(set(items))

# Set operations
common = set1 & set2  # intersection
all_items = set1 | set2  # union
only_in_first = set1 - set2  # difference
exclusive = set1 ^ set2  # symmetric difference

# Fast membership testing
if item in valid_items:  # O(1) instead of O(n)
    process(item)
```

## Combining Different Collection Types

### Common Combinations
```python
# List of dictionaries (records)
users = [
    {"name": "Alice", "age": 30},
    {"name": "Bob", "age": 25}
]

# Dictionary of lists (grouped data)
groups = {
    "admin": ["alice", "bob"],
    "user": ["charlie", "diana"]
}

# Set of tuples (unique coordinate pairs)
coordinates = {(0, 0), (1, 2), (3, 4)}

# Dictionary with tuple keys
cache = {
    ("user", 123): {"name": "Alice"},
    ("post", 456): {"title": "Hello"}
}
```

### Choosing Nested Structures

Use list of dicts when each item has multiple attributes (database records). Use dict of lists when grouping items by category. Use dict of dicts for hierarchical or tree-like data. Keep nesting shallow (2-3 levels max) for maintainability.

## Data Transformation Pipelines

### Chaining Operations
```python
# Functional pipeline style
result = (
    [x for x in data if x > 0]  # filter
    |> (lambda xs: [x**2 for x in xs])  # transform
    |> sum  # reduce
)

# More common approach
filtered = [x for x in data if x > 0]
squared = [x**2 for x in filtered]
total = sum(squared)

# Generator pipeline for memory efficiency
from itertools import islice

def process_large_file(filename):
    with open(filename) as f:
        lines = (line.strip() for line in f)  # generator
        non_empty = (line for line in lines if line)
        processed = (transform(line) for line in non_empty)
        return list(islice(processed, 1000))  # first 1000
```

### Map, Filter, Reduce
```python
from functools import reduce

# map: transform each element
doubled = list(map(lambda x: x * 2, numbers))
# Comprehension preferred: [x * 2 for x in numbers]

# filter: select elements
positives = list(filter(lambda x: x > 0, numbers))
# Comprehension preferred: [x for x in numbers if x > 0]

# reduce: combine elements
total = reduce(lambda a, b: a + b, numbers)
# Built-in preferred: sum(numbers)
```

Comprehensions are generally more readable than map/filter in Python. Use map/filter when you already have named functions to apply.

## Nested Data Structures

### Accessing Nested Data
```python
# Safe nested access
user = data.get("user", {}).get("profile", {}).get("name")

# With default
from collections import defaultdict

nested = defaultdict(lambda: defaultdict(list))
nested["category"]["subcategory"].append(item)

# Flattening nested lists
nested_list = [[1, 2], [3, 4], [5]]
flat = [item for sublist in nested_list for item in sublist]

# Deep copy for nested structures
import copy
deep_copied = copy.deepcopy(original)
```

### Working with JSON-Like Data
```python
# Navigating nested JSON
data = {
    "users": [
        {"id": 1, "name": "Alice", "posts": [{"title": "Hello"}]},
        {"id": 2, "name": "Bob", "posts": []}
    ]
}

# Extract all post titles
titles = [
    post["title"]
    for user in data["users"]
    for post in user.get("posts", [])
]

# Build index from nested data
user_index = {user["id"]: user for user in data["users"]}
```

## Collection Best Practices

**Use appropriate types:** Don't use lists for membership testing or dictionaries when you only need values. Match the collection to your access patterns.

**Avoid premature optimization:** Start with simple structures (lists, dicts). Optimize only when profiling shows a bottleneck.

**Consider immutability:** Use tuples instead of lists when data shouldn't change. Use `frozenset` for immutable sets.

**Prefer comprehensions:** List/dict/set comprehensions are more readable and often faster than loops or map/filter.

**Use generators for large data:** Generator expressions save memory when you don't need all values at once.

**Document nested structures:** Add type hints or docstring examples for complex nested collections.

**Watch for mutation:** Remember that lists and dicts are mutable. Be careful with default arguments and aliasing.
```python
# Bad: mutable default argument
def add_item(item, items=[]):  # Don't do this
    items.append(item)
    return items

# Good: use None
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

**Use named tuples or dataclasses:** For structured data with named fields, these are clearer than plain tuples or dicts.
```python
from collections import namedtuple

Point = namedtuple("Point", ["x", "y"])
p = Point(10, 20)
print(p.x)  # More readable than p[0]
```
