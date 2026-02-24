# Comprehensions
Comprehensions provide a concise way to create lists, dictionaries, sets, and generators in Python. They offer readable, efficient alternatives to traditional loops for transforming and filtering data.

## What are Comprehensions?

Comprehensions are syntactic constructs that let you build collections from existing iterables in a single, readable line. They combine iteration, conditional logic, and transformation into compact expressions.

## List Comprehensions

### Basic Syntax
```python
# Traditional approach
squares = []
for x in range(5):
    squares.append(x ** 2)

# List comprehension
squares = [x ** 2 for x in range(5)]  # [0, 1, 4, 9, 16]
```

The basic syntax: `[expression for item in iterable]`

### Filtering with Conditions

Add `if` clauses to filter elements:
```python
# Get even numbers
evens = [x for x in range(10) if x % 2 == 0]  # [0, 2, 4, 6, 8]

# Filter strings by length
words = ['cat', 'elephant', 'dog', 'butterfly']
long_words = [word for word in words if len(word) > 5]  # ['elephant', 'butterfly']
```

### Transforming Data

Apply operations to each element:
```python
# Convert to uppercase
names = ['alice', 'bob', 'charlie']
upper_names = [name.upper() for name in names]  # ['ALICE', 'BOB', 'CHARLIE']

# Multiple transformations
prices = [10, 20, 30]
final_prices = [price * 1.1 for price in prices]  # [11.0, 22.0, 33.0]
```

### Nested List Comprehensions

Flatten or process nested structures:
```python
# Flatten a 2D list
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flat = [num for row in matrix for num in row]  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Create a 2D list
grid = [[i * j for j in range(3)] for i in range(3)]
# [[0, 0, 0], [0, 1, 2], [0, 2, 4]]
```

## Dictionary Comprehensions

### Creating Dictionaries from Sequences
```python
# Basic syntax: {key_expr: value_expr for item in iterable}
numbers = [1, 2, 3, 4]
squared = {x: x ** 2 for x in numbers}  # {1: 1, 2: 4, 3: 9, 4: 16}

# From two lists
keys = ['name', 'age', 'city']
values = ['Alice', 30, 'NYC']
person = {k: v for k, v in zip(keys, values)}
# {'name': 'Alice', 'age': 30, 'city': 'NYC'}
```

### Filtering Dictionaries
```python
scores = {'Alice': 85, 'Bob': 92, 'Charlie': 78, 'Diana': 95}

# Filter by condition
high_scores = {k: v for k, v in scores.items() if v >= 90}
# {'Bob': 92, 'Diana': 95}
```

### Transforming Keys and Values
```python
# Transform values
prices = {'apple': 1.0, 'banana': 0.5, 'orange': 0.75}
discounted = {k: v * 0.9 for k, v in prices.items()}
# {'apple': 0.9, 'banana': 0.45, 'orange': 0.675}

# Transform keys
original = {1: 'one', 2: 'two', 3: 'three'}
string_keys = {str(k): v for k, v in original.items()}
# {'1': 'one', '2': 'two', '3': 'three'}
```

## Set Comprehensions

### Creating Sets from Sequences
```python
# Basic syntax: {expression for item in iterable}
numbers = [1, 2, 2, 3, 3, 4]
unique_squares = {x ** 2 for x in numbers}  # {1, 4, 9, 16}
```

### Deduplication with Comprehensions
```python
# Remove duplicates with transformation
words = ['Hello', 'hello', 'WORLD', 'world']
unique_lower = {word.lower() for word in words}  # {'hello', 'world'}

# Filter and deduplicate
numbers = [1, -2, 3, -4, 5, -6, 3, 1]
positive_unique = {x for x in numbers if x > 0}  # {1, 3, 5}
```

## Generator Expressions (vs. List Comprehensions)

Generator expressions use parentheses instead of brackets and produce values lazily:
```python
# List comprehension (creates entire list in memory)
squares_list = [x ** 2 for x in range(1000000)]

# Generator expression (produces values on demand)
squares_gen = (x ** 2 for x in range(1000000))

# Use in iteration
for square in squares_gen:
    if square > 100:
        break
```

Key differences:
- Generators save memory for large datasets
- Generators can only be iterated once
- List comprehensions are faster for small datasets or multiple iterations

## When to Use Comprehensions

### Readability vs. Complexity

**Use comprehensions when:**
- The operation is simple and fits naturally on one or two lines
- The logic is straightforward filtering or transformation
- The code becomes more readable than a loop
```python
# Good: Simple and clear
evens = [x for x in range(20) if x % 2 == 0]
```

**Avoid comprehensions when:**
- Multiple conditions make the expression hard to read
- Complex logic requires multiple transformations
- You need error handling or debugging
```python
# Bad: Too complex
result = [process(x) if validate(x) and x > 0 else default(x) 
          for x in data if pre_check(x) and x not in exclusions]

# Better: Use a regular loop
result = []
for x in data:
    if pre_check(x) and x not in exclusions:
        result.append(process(x) if validate(x) and x > 0 else default(x))
```

### Performance Considerations

Comprehensions are generally faster than equivalent loops because:
- They're optimized at the bytecode level
- They avoid repeated function calls (like `append()`)

However:
- Use generator expressions for large datasets to save memory
- Don't sacrifice readability for minor performance gains
- Profile your code before optimizing
```python
import timeit

# Comprehension (faster)
timeit.timeit('[x**2 for x in range(1000)]', number=10000)

# Loop (slower)
timeit.timeit('result = []\nfor x in range(1000):\n    result.append(x**2)', 
              number=10000)
```
