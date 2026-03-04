# File Operations Best Practices

This guide covers essential best practices for file operations in Python 3. Proper file handling prevents resource leaks, data corruption, and security vulnerabilities. Following these patterns ensures robust, maintainable code.

## Always Use Context Managers

Context managers automatically close files, even when errors occur.
```python
# Good: Automatic cleanup
with open('data.txt', 'r') as f:
    content = f.read()
# File is closed here, even if an exception occurred

# Bad: Manual cleanup required
f = open('data.txt', 'r')
content = f.read()
f.close()  # Won't execute if an error occurs above
```

Multiple files can be managed together:
```python
with open('input.txt', 'r') as infile, open('output.txt', 'w') as outfile:
    outfile.write(infile.read())
```

## Handle File Not Found Errors

Check for missing files before operations fail unexpectedly.
```python
from pathlib import Path

# Method 1: Try-except
try:
    with open('config.json', 'r') as f:
        data = f.read()
except FileNotFoundError:
    print("Config file not found, using defaults")
    data = get_default_config()

# Method 2: Check first (pathlib)
config_file = Path('config.json')
if config_file.exists():
    with open(config_file, 'r') as f:
        data = f.read()
else:
    data = get_default_config()
```

## Handle Permission Errors

Users may lack read/write permissions on files or directories.
```python
try:
    with open('/etc/protected.conf', 'w') as f:
        f.write(data)
except PermissionError:
    print("Error: Insufficient permissions to write file")
    # Log error or write to alternate location
except OSError as e:
    print(f"OS error occurred: {e}")
```

## Verify File Existence

Use `pathlib` for modern, readable path operations.
```python
from pathlib import Path

file_path = Path('data/results.csv')

# Check existence
if file_path.exists():
    print("File found")

# Check if it's a file (not a directory)
if file_path.is_file():
    print("Valid file")

# Check if it's a directory
if file_path.is_dir():
    print("Is a directory")

# Create parent directories if needed
file_path.parent.mkdir(parents=True, exist_ok=True)
```

## Resource Cleanup Patterns

Ensure resources are released properly in all scenarios.
```python
# Pattern 1: Context manager (preferred)
with open('data.txt', 'r') as f:
    process(f)

# Pattern 2: Try-finally (when context manager unavailable)
f = open('data.txt', 'r')
try:
    process(f)
finally:
    f.close()

# Pattern 3: Multiple resources
resources = []
try:
    f1 = open('input.txt', 'r')
    resources.append(f1)
    f2 = open('output.txt', 'w')
    resources.append(f2)
    process(f1, f2)
finally:
    for resource in resources:
        resource.close()
```

## File Operation Error Handling

Catch specific exceptions and handle them appropriately.
```python
import errno

try:
    with open('data.txt', 'r') as f:
        content = f.read()
except FileNotFoundError:
    # File doesn't exist
    print("File not found")
except PermissionError:
    # No permission to access
    print("Permission denied")
except IsADirectoryError:
    # Path is a directory, not a file
    print("Expected file, found directory")
except OSError as e:
    # Catch other OS-level errors
    if e.errno == errno.ENOSPC:
        print("No space left on device")
    else:
        print(f"OS error: {e}")
except Exception as e:
    # Unexpected errors
    print(f"Unexpected error: {e}")
```

## Performance Considerations

Optimize file operations for speed and memory efficiency.
```python
# Read large files line-by-line (memory efficient)
with open('large_file.txt', 'r') as f:
    for line in f:  # Iterates without loading entire file
        process(line)

# Read in chunks for binary files
with open('large_binary.dat', 'rb') as f:
    while chunk := f.read(8192):  # 8KB chunks
        process(chunk)

# Use buffering for frequent small writes
with open('output.txt', 'w', buffering=8192) as f:
    for item in data:
        f.write(item)

# Avoid repeated file operations
# Bad: Opens file 1000 times
for item in data:
    with open('log.txt', 'a') as f:
        f.write(f"{item}\n")

# Good: Opens file once
with open('log.txt', 'a') as f:
    for item in data:
        f.write(f"{item}\n")
```

## Security Considerations

Protect against path traversal, injection, and data exposure.
```python
from pathlib import Path
import os

# Prevent path traversal attacks
def safe_join(base_dir, user_filename):
    base = Path(base_dir).resolve()
    full_path = (base / user_filename).resolve()
    
    # Ensure the path stays within base_dir
    if not str(full_path).startswith(str(base)):
        raise ValueError("Path traversal detected")
    return full_path

# Use this for user-provided filenames
safe_path = safe_join('/var/app/uploads', user_input)

# Set restrictive file permissions
with open('sensitive.txt', 'w') as f:
    f.write(secret_data)
os.chmod('sensitive.txt', 0o600)  # Owner read/write only

# Avoid race conditions with exclusive creation
try:
    with open('lockfile', 'x') as f:  # 'x' = exclusive creation
        f.write('locked')
except FileExistsError:
    print("Lock already exists")

# Sanitize filenames from user input
def sanitize_filename(filename):
    # Remove path separators and dangerous characters
    safe_name = os.path.basename(filename)
    safe_name = "".join(c for c in safe_name if c.isalnum() or c in '._- ')
    return safe_name.strip()
```
