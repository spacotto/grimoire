# Introduction to File Operations

File operations are fundamental to programming. They let you save data, load configurations, process logs, and exchange information between programs. This guide covers the essentials of working with files in Python—what they are, why they matter, and how to use them effectively.

## What is File I/O?

File I/O (Input/Output) refers to reading data from files and writing data to files. It's how your program interacts with persistent storage on disk.

**Input**: Reading data from a file into your program
**Output**: Writing data from your program to a file
```python
# Reading (Input)
with open('data.txt', 'r') as f:
    content = f.read()

# Writing (Output)
with open('output.txt', 'w') as f:
    f.write('Hello, world!')
```

## Why File Operations Matter

File operations enable programs to:

- **Persist data** beyond program execution
- **Process large datasets** that don't fit in memory
- **Share information** between different programs and systems
- **Log events** for debugging and monitoring
- **Load configurations** to customize behavior
- **Generate reports** and exports for users

Without file I/O, every program would start from scratch each time it runs.

## File Paths and Locations

A file path tells Python where to find a file on your system.

**Absolute paths** specify the complete location from the root directory:
```python
'/home/user/documents/data.txt'  # Linux/Mac
'C:\\Users\\User\\Documents\\data.txt'  # Windows
```

**Relative paths** are relative to your current working directory:
```python
'data.txt'  # Same directory
'../data.txt'  # Parent directory
'files/data.txt'  # Subdirectory
```

Use `pathlib` for cross-platform compatibility:
```python
from pathlib import Path

file_path = Path('data') / 'input.txt'
```

## Text Files vs. Binary Files

**Text files** contain human-readable characters (`.txt`, `.csv`, `.json`, `.py`):
```python
with open('notes.txt', 'r') as f:  # 'r' for read mode
    text = f.read()
```

**Binary files** contain raw bytes (`.jpg`, `.pdf`, `.exe`, `.zip`):
```python
with open('image.jpg', 'rb') as f:  # 'rb' for read binary
    data = f.read()
```

Use text mode for text files and binary mode for everything else.

## File Operations Overview

### Opening Files

Always use `with` statements—they automatically close files:
```python
with open('file.txt', 'r') as f:
    content = f.read()
# File is automatically closed here
```

**Common modes**:
- `'r'` — read (default)
- `'w'` — write (overwrites existing file)
- `'a'` — append (adds to end of file)
- `'x'` — exclusive creation (fails if file exists)
- `'rb'`, `'wb'` — binary modes

### Reading Files
```python
# Read entire file
with open('file.txt', 'r') as f:
    content = f.read()

# Read line by line (memory efficient)
with open('file.txt', 'r') as f:
    for line in f:
        print(line.strip())

# Read all lines into a list
with open('file.txt', 'r') as f:
    lines = f.readlines()
```

### Writing Files
```python
# Write (overwrites)
with open('output.txt', 'w') as f:
    f.write('First line\n')
    f.write('Second line\n')

# Append (preserves existing content)
with open('log.txt', 'a') as f:
    f.write('New log entry\n')
```

### Checking File Existence
```python
from pathlib import Path

if Path('file.txt').exists():
    print('File exists')

if Path('directory').is_dir():
    print('Directory exists')
```

## Common File Operation Pitfalls

### 1. Forgetting to Close Files

**Problem**: Files left open can cause data loss or locks
```python
# Bad
f = open('file.txt', 'r')
content = f.read()
# Forgot to close!
```

**Solution**: Always use `with` statements
```python
# Good
with open('file.txt', 'r') as f:
    content = f.read()
```

### 2. Using the Wrong Mode

**Problem**: Opening a binary file in text mode corrupts data
```python
# Bad - corrupts image
with open('photo.jpg', 'r') as f:
    data = f.read()
```

**Solution**: Use binary mode for binary files
```python
# Good
with open('photo.jpg', 'rb') as f:
    data = f.read()
```

### 3. Overwriting Files Accidentally

**Problem**: Using `'w'` mode deletes existing content immediately
```python
# Bad - deletes everything in file.txt before you even write
with open('important.txt', 'w') as f:
    # Oops, file is already empty!
    f.write('new content')
```

**Solution**: Use `'x'` mode to prevent overwriting or check existence first
```python
# Good
from pathlib import Path
if not Path('important.txt').exists():
    with open('important.txt', 'w') as f:
        f.write('new content')
```

### 4. Not Handling Missing Files

**Problem**: Program crashes if file doesn't exist
```python
# Bad
with open('config.txt', 'r') as f:  # FileNotFoundError if missing
    config = f.read()
```

**Solution**: Check existence or handle exceptions
```python
# Good
from pathlib import Path

if Path('config.txt').exists():
    with open('config.txt', 'r') as f:
        config = f.read()
else:
    config = get_default_config()
```

### 5. Encoding Issues

**Problem**: Special characters appear garbled
```python
# Bad - uses system default encoding
with open('file.txt', 'r') as f:
    content = f.read()
```

**Solution**: Explicitly specify UTF-8 encoding
```python
# Good
with open('file.txt', 'r', encoding='utf-8') as f:
    content = f.read()
```

### 6. Reading Large Files Entirely

**Problem**: Loading huge files into memory causes crashes
```python
# Bad - loads entire 10GB file into memory
with open('huge_log.txt', 'r') as f:
    content = f.read()
```

**Solution**: Process line by line
```python
# Good - processes one line at a time
with open('huge_log.txt', 'r') as f:
    for line in f:
        process(line)
```
