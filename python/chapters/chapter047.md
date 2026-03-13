# Introduction to Modules

As programs grow, keeping all code in a single file becomes impractical. Python's module system solves this by letting you split code into separate, reusable files. This note covers what modules are, why they matter, and how Python locates and loads them at runtime.

## What are Modules?

A **module** is any Python file (`.py`) that contains definitions and statements — functions, classes, variables, or runnable code — which can be imported and reused in other scripts.
```python
# greet.py  ← this file *is* a module
def say_hello(name):
    return f"Hello, {name}!"

# main.py
import greet

print(greet.say_hello("Silvia"))  # Hello, Silvia!
```

>[!NOTE]
>Once imported, every name defined in `greet.py` is available through the `greet` namespace.

## Why Use Modules?

| Benefit | What it means in practice |
|---|---|
| **Reusability** | Write a function once; import it anywhere. |
| **Maintainability** | Smaller, focused files are easier to read and fix. |
| **Namespace isolation** | Each module has its own scope — no accidental name collisions. |
| **Selective imports** | Load only what you need with `from module import name`. |
| **Collaboration** | Different team members can own different modules cleanly. |

## Module Files (`.py`)

Any `.py` file is a valid module. There are no special keywords needed — saving the file is enough.

```python
project/
├── main.py          # entry point
├── utils.py         # utility functions  ← module
└── data_handler.py  # data logic         ← module
```
