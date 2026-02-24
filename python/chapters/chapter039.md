# Command-Line Arguments in Python
Command-line arguments let users pass input to a Python script at runtime, directly from the terminal. This makes scripts flexible and reusable — no hardcoding required. Python's built-in `sys` module provides straightforward access to these arguments through `sys.argv`.

## The `sys` Module

The `sys` module is part of Python's standard library. It gives access to variables and functions tied to the Python interpreter itself.

```python
import sys
```

>[!NOTE]
>No installation needed — just import it.

## Understanding `sys.argv`

`sys.argv` is a **list** that holds all command-line arguments passed to a script.

- It is always a list of **strings**
- It is always available after importing `sys`
- It always contains **at least one element** (the script name)

```python
import sys
print(sys.argv)
```

Running `python3 script.py hello 42` outputs:

```
['script.py', 'hello', '42']
```

>[!IMPORTANT]
>`sys.argv` is a list containing command-line arguments passed to your Python script, where `sys.argv[0]` is always the script name itself and `sys.argv[1:]` contains the actual arguments. For example, running `python script.py arg1 arg2` results in `sys.argv = ['script.py', 'arg1', 'arg2']`. The program name is simply the identifier of what's being executed, while the arguments are the data you pass to modify the script's behavior. All values in `sys.argv` are strings, and you can check `len(sys.argv)` to see how many total items exist (script name + arguments).

## Accessing Command-Line Arguments

Use standard list indexing to access individual arguments.

```python
import sys

first_arg = sys.argv[1]
second_arg = sys.argv[2]

print(first_arg)   # hello
print(second_arg)  # 42
```

>[!NOTE]
>Arguments are always strings. Use `int()`, `float()`, etc. to convert them when needed.

```python
number = int(sys.argv[1])
```

## Program Name vs. Arguments

`sys.argv[0]` is always the **script's name**, not a user-provided argument.

| Index | Value |
|-------|-------|
| `sys.argv[0]` | Script name (`script.py`) |
| `sys.argv[1]` | First user argument |
| `sys.argv[2]` | Second user argument |
| `...` | `...` |

```python
import sys

script_name = sys.argv[0]
user_args   = sys.argv[1:]  # everything after the script name

print(f"Script: {script_name}")
print(f"Arguments: {user_args}")
```

## Processing Multiple Arguments

Use a loop or list slicing to handle a variable number of arguments.

```python
import sys

args = sys.argv[1:]  # skip the script name

for arg in args:
    print(f"Got: {arg}")
```

>[!WARNING]
>Always check `len(sys.argv)` before accessing an index to avoid `IndexError`.

```python
import sys

if len(sys.argv) < 2:
    print("Usage: python3 script.py <name>")
    sys.exit(1)

name = sys.argv[1]
print(f"Hello, {name}!")
```

## Command-Line Data Processing

A practical example: summing a list of numbers passed as arguments.

```python
import sys

if len(sys.argv) < 2:
    print("Usage: python3 sum.py <num1> <num2> ...")
    sys.exit(1)

numbers = [float(arg) for arg in sys.argv[1:]]
total   = sum(numbers)

print(f"Sum: {total}")
```

Running `python3 sum.py 10 20 30.5` outputs:

```
Sum: 60.5
```

>[!NOTE]
>This pattern — validate input, convert types, process data — applies to most command-line scripts.

