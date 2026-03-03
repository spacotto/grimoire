# Reading Files

Reading files is a fundamental operation in Python. This guide covers the essential concepts and methods for working with file input, from opening files to reading their contents in various ways.

## The open() Function

The `open()` function creates a file object and is the standard way to work with files in Python.
```python
file = open('example.txt', 'r')
```

>[!IMPORTANT]
>Basic syntax: `open(filename, mode)`

## File Modes

Common file modes determine how you can interact with a file.

| Syntax | Mode | Description |
| :--- | :--- | :--- |
| `'r'` | Read mode (default) | File must exist |
| `'w'` | Write mode | Creates new file or overwrites existing|
| `'a'` | Append mode | Adds to end of existing file |
| `'r+'` | Read and write mode | File must exist |

Example:
```python
file = open('data.txt', 'r')   # reading only
file = open('output.txt', 'w') # writing only
```

## Reading Entire Files (read())

The `read()` method returns the entire file contents as a single string.
```python
file = open('story.txt', 'r')
content = file.read()
print(content)
file.close()
```

You can specify the number of characters to read:
```python
first_100_chars = file.read(100)
```

## Reading Line by Line (readline())

The `readline()` method reads one line at a time, including the newline character.
```python
file = open('data.txt', 'r')
line1 = file.readline()
line2 = file.readline()
file.close()
```

Returns an empty string when reaching the end of the file.

## Reading All Lines (readlines())

The `readlines()` method returns a list where each element is a line from the file.
```python
file = open('items.txt', 'r')
lines = file.readlines()
# lines = ['first line\n', 'second line\n', 'third line\n']
file.close()
```

## File Objects and Iteration

File objects are iterable, making it easy to loop through lines without loading the entire file into memory.
```python
file = open('log.txt', 'r')
for line in file:
    print(line.strip())
file.close()
```

This approach is memory-efficient for large files.

## Closing Files with close()

Always close files when finished to free up system resources.
```python
file = open('data.txt', 'r')
content = file.read()
file.close()
```

**Better approach:** Use a context manager (with statement) to automatically close files:
```python
with open('data.txt', 'r') as file:
    content = file.read()
# File automatically closed after this block
```

## File Encoding

Specify encoding to handle different character sets correctly. UTF-8 is the recommended default.
```python
file = open('international.txt', 'r', encoding='utf-8')
```

Common encodings:
- `'utf-8'` - Universal, supports all characters
- `'ascii'` - Basic English characters only
- `'latin-1'` - Western European characters
```python
with open('data.txt', 'r', encoding='utf-8') as file:
    content = file.read()
```

Explicit encoding prevents issues with special characters and ensures consistent behavior across different systems.
