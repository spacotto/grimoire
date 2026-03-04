# Writing Files in Python

Writing to files is a fundamental operation in Python programming. This guide covers the essential methods and best practices for creating and modifying files, including write modes, buffer management, and file permissions.

## Opening Files for Writing

Use the `open()` function with a mode parameter to open files for writing:
```python
file = open('output.txt', 'w')
file.write('Hello, world!')
file.close()
```

Always close files after writing. Use a **context manager** to ensure proper cleanup:
```python
with open('output.txt', 'w') as file:
    file.write('Hello, world!')
# File automatically closes here
```

## Write Mode vs. Append Mode

Python provides two primary modes for writing:

**Write mode (`'w'`)**: Creates a new file or overwrites existing content

```python
# Write mode - replaces all content
with open('log.txt', 'w') as file:
    file.write('New log entry\n')
```

**Append mode (`'a'`)**: Adds content to the end of an existing file

```python
# Append mode - adds to existing content
with open('log.txt', 'a') as file:
    file.write('Additional entry\n')
```

## The write() Method

The `write()` method writes a string to the file and returns the number of characters written:

```python
with open('data.txt', 'w') as file:
    chars_written = file.write('First line\n')
    print(f'Wrote {chars_written} characters')
```

>[!NOTE]
>`write()` does not add newlines automatically. Include `\n` explicitly when needed.

## The writelines() Method

The `writelines()` method writes a list of strings to the file:
```python
lines = ['Line 1\n', 'Line 2\n', 'Line 3\n']

with open('output.txt', 'w') as file:
    file.writelines(lines)
```

>[!NOTE]
>Like `write()`, this method does not add newlines automatically. Include them in your strings.

## Overwriting vs. Appending

Understanding the difference prevents accidental data loss:
```python
# Overwriting - destroys existing content
with open('data.txt', 'w') as file:
    file.write('This replaces everything\n')

# Appending - preserves existing content
with open('data.txt', 'a') as file:
    file.write('This is added to the end\n')
```

To read and modify existing content, use `'r+'` mode:
```python
with open('data.txt', 'r+') as file:
    content = file.read()
    file.write('\nAppended text')
```

## Flushing Buffers

Python buffers writes for performance. Force immediate writing with `flush()`:
```python
with open('log.txt', 'w') as file:
    file.write('Critical message\n')
    file.flush()  # Ensures data is written to disk immediately
```

The context manager automatically flushes when closing, but manual flushing is useful for:
- Long-running processes
- Real-time logging
- Ensuring data persistence before potential crashes

## File Permissions

Python respects system file permissions. Handle permission errors gracefully:
```python
try:
    with open('/protected/file.txt', 'w') as file:
        file.write('Data')
except PermissionError:
    print('Error: insufficient permissions to write file')
```

Set file permissions on Unix-like systems using `os.chmod()`:
```python
import os

# Create file
with open('private.txt', 'w') as file:
    file.write('Sensitive data')

# Set read/write for owner only (0o600)
os.chmod('private.txt', 0o600)
```

Common permission modes:
- `0o644`: Owner read/write, others read-only
- `0o600`: Owner read/write, no access for others
- `0o755`: Owner all permissions, others read/execute
