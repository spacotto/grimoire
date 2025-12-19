# About Strings
A string is a **sequence of characters enclosed in quotes**. Strings are **immutable**, meaning they cannot be changed after creation.

Example:
```
single = 'Hello'
double = "World"
triple = '''Multi
line'''
```

## Creating Strings
You can use single quotes, double quotes, or triple quotes (for multi-line strings):
```
name = "Alice"
message = 'Hello, World!'
paragraph = """This spans
multiple lines"""
```

## String Operations
### Concatenation
Join strings with `+`
```
greeting = "Hello" + " " + "World"  # "Hello World"
```

### Repetition
Repeat strings with `*`
```
pythonecho = "Ha" * 3  # "HaHaHa"
```

### Length
Get string length with `len()`
```
pythonlen("Python")  # 6
```

### Indexing and Slicing
Access characters by position (zero-indexed):
```
text = "Python"
text[0]      # 'P' (first character)
text[-1]     # 'n' (last character)
text[1:4]    # 'yth' (slice from index 1 to 3)
text[:3]     # 'Pyt' (first 3 characters)
text[3:]     # 'hon' (from index 3 to end)
```

## Common String Methods
### Case conversion
Converts to uppercase:
```
upper()
```

Converts to lowercase:
```
lower()
```

Capitalizes first letter:
```
capitalize()
```

Capitalizes each word
```
title()
```

### Searching
