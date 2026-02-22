# Program Structure and Execution
Python programs are plain text files interpreted top-to-bottom by the CPython interpreter. Understanding how Python loads and runs code — modules, scripts, entry points, and execution context — is essential for writing reusable, well-structured programs.

## How Python Programs Execute

When you run `python3 file_name.py`, the interpreter:

1. Reads the source file and compiles it to bytecode (`.pyc` cached in `__pycache__/`).
2. Executes the bytecode top-to-bottom in a fresh module namespace.
3. Calls any top-level statements immediately (imports, assignments, function calls).

Functions and classes are **defined** at load time but not **called** unless explicitly invoked.

```python
# Everything at the top level runs immediately
print("runs on import")  # executes even if this file is imported

def greet():
    print("called only when invoked")
```

## The `if __name__ == "__main__":` Pattern

The most common Python **entry-point pattern**. Code inside this block runs **only** when the file is executed directly, not when imported as a module.

```python
def main():
    print("Program started")

if __name__ == "__main__":
    main()
```

**Why it matters:**
- Keeps scripts importable without triggering side effects.
- Enables testing individual modules without running the full program.
- Separates reusable logic from execution logic.

## Understanding `__name__` and Module Execution

Every Python file has a built-in `__name__` variable:

| Scenario | `__name__` value |
|---|---|
| Run directly (`python3 file.py`) | `"__main__"` |
| Imported (`import file`) | `"file"` (the module name) |

```python
# module_a.py
print(f"I am: {__name__}")
```

```bash
$ python3 module_a.py   # prints: I am: __main__
$ python3 -c "import module_a"  # prints: I am: module_a
```

Python also supports running packages as scripts:

```bash
python3 -m module_name   # runs module's __main__.py or sets __name__ = "__main__"
```

## Shebang Lines and Script Permissions

A **shebang** (`#!`) on the first line tells the OS which interpreter to use when the file is executed directly from the shell.

```python
#!/usr/bin/env python3
```

- `#!/usr/bin/env python3` — preferred; resolves `python3` from `$PATH` (portable).
- `#!/usr/bin/python3` — hardcoded path; less portable.

**Make the script executable:**

```bash
chmod +x script.py
./script.py        # runs without typing `python3`
```

Shebang lines are ignored by the Python interpreter itself — they only matter to the OS shell.

---

## When to Use Main Blocks vs. Functions

| Situation | Recommendation |
|---|---|
| One-off script, no reuse | `if __name__ == "__main__":` with inline code is fine |
| Script you might import or test | Wrap logic in `main()`, call from `__name__` block |
| Package entry point | Define `main()` in a module, reference in `pyproject.toml` `[scripts]` |
| Shared utility code | No main block; just define functions/classes |

**Preferred structure for most scripts:**

```python
#!/usr/bin/env python3

def process(data):
    ...

def main():
    data = load()
    result = process(data)
    print(result)

if __name__ == "__main__":
    main()
```

This makes the script directly runnable, importable, and testable.
