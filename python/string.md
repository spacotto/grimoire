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

Capitalizes each word:
```
title()
```

### Searching
Returns index of first occurrence, -1 if not found:
```
find(substring)
```

Counts occurrences:
```
count(substring)
```

Checks if string starts with prefix:
```
startswith(prefix)
```

Checks if string ends with suffix:
```
endswith(suffix)
```

### Modification
Removes whitespace from both ends:
```
strip()
```

Replaces all occurrences:
```
replace(old, new)
```

Splits string into list:
```
split(separator)
```

Joins list into string:
```
join(iterable)
```

## String Formatting
f-strings (recommended, Python 3.6+):
```
pythonname = "Alice"
age = 30
f"My name is {name} and I'm {age}"   # "My name is Alice and I'm 30"
f"{10 / 3:.2f}"                      # "3.33" (2 decimal places)
```

`format()` method:
```
python"Hello, {}!".format("World")              # "Hello, World!"
"{0} {1}".format("Hello", "World")              # "Hello World"
"{name} is {age}".format(name="Bob", age=25)
```

%-formatting (older style):
```
python"Hello, %s!" % "World"           # "Hello, World!"
"%d items" % 5                         # "5 items"
```

## Escape Characters
Use backslash `\` for special characters:
```
"Hello\nWorld"   # newline
"Tab\there"      # tab
"Quote: \"Hi\""  # escaped quote
"Path: C:\\dir"  # backslash (doubled)
```

Use raw strings to ignore escapes:
```
r"C:\new\path"  # backslash treated literally
```

## Checking String Content
```
"123".isdigit()      # True (all digits)
"abc".isalpha()      # True (all letters)
"abc123".isalnum()   # True (letters or digits)
"   ".isspace()      # True (all whitespace)
```
