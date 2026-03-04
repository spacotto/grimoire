# Standard Streams in Python

Standard streams are pre-established communication channels between a program and its environment. Python provides three primary streams through the `sys` module: `stdin` for input, `stdout` for normal output, and `stderr` for error messages. Understanding these streams is essential for building command-line tools, processing piped data, and properly handling program output.

## Understanding Standard I/O

Every Python process automatically receives three file-like objects:

- **stdin** (standard input) - file descriptor 0
- **stdout** (standard output) - file descriptor 1  
- **stderr** (standard error) - file descriptor 2

>[!NOTE]
>These streams connect your program to the terminal, files, or other programs through redirection and pipes.

## Standard Input (stdin)

`stdin` receives input data. By default, it reads from the keyboard, but can read from files or other programs.
```python
import sys

# Read a single line
line = sys.stdin.readline()

# Read all input at once
data = sys.stdin.read()
```

## Standard Output (stdout)

`stdout` is the primary output channel. The `print()` function writes to `stdout` by default.
```python
import sys

# These are equivalent
print("Hello")
sys.stdout.write("Hello\n")
```

## Standard Error (stderr)

`stderr` is dedicated to error messages and diagnostics. It's separate from `stdout`, so errors aren't mixed with normal output.
```python
import sys

sys.stderr.write("Error: file not found\n")
print("Error occurred", file=sys.stderr)
```

## Reading from sys.stdin
```python
import sys

# Line by line (memory efficient)
for line in sys.stdin:
    process(line.strip())

# Read entire input
content = sys.stdin.read()

# Read single line
user_input = sys.stdin.readline().strip()
```

## Writing to sys.stdout
```python
import sys

# Direct write (no automatic newline)
sys.stdout.write("Output")

# Using print (adds newline by default)
print("Output", end='')  # suppress newline

# Flush buffer immediately
sys.stdout.flush()
```

## Writing to sys.stderr
```python
import sys

# Write error messages
sys.stderr.write("Warning: deprecated function\n")

# Using print with file parameter
print("Error:", error_msg, file=sys.stderr)

# Formatting errors
sys.stderr.write(f"Failed to process {filename}\n")
```

## Stream Redirection

**Command line examples:**
```bash
# Redirect stdout to file
python script.py > output.txt

# Redirect stderr to file
python script.py 2> errors.txt

# Redirect both
python script.py > output.txt 2> errors.txt

# Pipe stdout to another program
python script.py | grep "pattern"

# Provide stdin from file
python script.py < input.txt
```

**In Python:**
```python
import sys
from io import StringIO

# Temporarily redirect stdout
old_stdout = sys.stdout
sys.stdout = StringIO()
print("Captured")
output = sys.stdout.getvalue()
sys.stdout = old_stdout
```

## When to Use Each Stream

**Use stdin when:**
- Processing piped data from other programs
- Reading configuration or input data
- Building filters or data processors

**Use stdout when:**
- Outputting program results
- Sending data to other programs via pipes
- Displaying primary program information

**Use stderr when:**
- Logging errors and warnings
- Displaying debug information
- Reporting progress that shouldn't mix with output

## Separating Normal Output from Errors

Proper stream separation enables better program composition:
```python
import sys

def process_file(filename):
    try:
        with open(filename) as f:
            data = f.read()
            # Normal output goes to stdout
            print(data.upper())
    except FileNotFoundError:
        # Errors go to stderr
        print(f"Error: {filename} not found", file=sys.stderr)
        sys.exit(1)
```

This allows users to:
```bash
# Save output, see errors on screen
python script.py > results.txt

# Save output and errors separately
python script.py > results.txt 2> errors.log

# Process output while seeing errors
python script.py 2> /dev/null | sort
```

**Best practices:**

- Always write errors to `stderr`, not `stdout`
- Keep `stdout` clean for data output
- Use `stderr` for logging and progress messages
- Flush streams when immediate output is needed
- Handle broken pipe errors when writing to closed streams
