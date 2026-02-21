# Introduction to Python

This guide covers the essentials to get started with Python. It begins with what Python is and the design principles behind it, then walks through setting up a working environment on Python 3.11.0+. The final two sections focus on writing clean, consistent code using the `PEP 8` style guide and enforcing those standards automatically with the `flake8` linter.

## What is Python

Python is a high-level, interpreted, general-purpose programming language created by **Guido van Rossum** and first released in 1991. It emphasises code readability and simplicity, making it one of the most popular languages for beginners and professionals alike.

**Key characteristics:**
- **Interpreted** — code runs line by line without a separate compilation step
- **Dynamically typed** — variable types are determined at runtime
- **Multi-paradigm** — supports procedural, object-oriented, and functional styles
- **Extensive standard library** — batteries included

**Common use cases:** web development, data science, machine learning, automation, scripting, and APIs.

## Python Philosophy and Design Principles

Python's design is guided by **The Zen of Python** (`PEP 20`), a set of 19 aphorisms. Access it anytime:

```python
import this
```

**Core principles:**

| Principle | Meaning |
|-----------|---------|
| Readability counts | Clear code is preferred over clever code |
| Explicit is better than implicit | Avoid hidden behavior |
| Simple is better than complex | Favor straightforward solutions |
| Errors should never pass silently | Handle exceptions explicitly |
| There should be one obvious way | One idiomatic approach is preferred |

These principles directly influence how Python code is written and reviewed.

## Setting Up Your Python Environment (`Python 3.11.0+`)

### Install Python

Download from [python.org](https://python.org) or use a version manager:

```bash
# macOS/Linux — using pyenv
brew install pyenv
pyenv install 3.11.0
pyenv global 3.11.0

# Verify installation
python --version   # Python 3.11.0
```

### Create a Virtual Environment

Isolate project dependencies to avoid conflicts:

```bash
# Create a virtual environment
python -m venv .venv

# Activate it
source .venv/bin/activate        # macOS/Linux
.venv\Scripts\activate           # Windows

# Deactivate when done
deactivate
```

### Install Packages

```bash
pip install <package-name>
pip install -r requirements.txt   # install from a list
pip freeze > requirements.txt     # save current dependencies
```

## Code Quality Standards and `PEP 8`

`PEP 8` is Python's official style guide. Following it ensures consistent, readable code across projects.

### Key Rules

#### Indentation
Use 4 spaces per level — never tabs.

```python
# ✅ Correct
def greet(name):
    print(f"Hello, {name}")

# ❌ Incorrect
def greet(name):
  print(f"Hello, {name}")
```

#### Line length
Maximum 79 characters per line.

#### Naming conventions

```python
my_variable = 10          # variables & functions: snake_case
MY_CONSTANT = 3.14        # constants: UPPER_SNAKE_CASE
class MyClass:            # classes: PascalCase
    pass
```

#### Blank lines
Two blank lines between top-level definitions; one between methods inside a class.

#### Imports
One import per line, grouped in order: standard library → third-party → local.

```python
import os
import sys

import requests

from mymodule import helper
```

#### Spaces
No extra spaces around operators inside brackets; spaces around binary operators.

```python
x = 5 + 3       # ✅ binary operator
list[1:3]        # ✅ slicing — no spaces
```

---

## Using the `flake8` Linter

`flake8` checks your code against `PEP 8` and catches common errors automatically.

### Install

```bash
pip install flake8
```

### Basic Usage

```bash
# Check a single file
flake8 main.py

# Check an entire directory
flake8 src/

# Show statistics
flake8 --statistics src/
```

### Example Output

```
main.py:5:1: E302 expected 2 blank lines, found 1
main.py:12:80: E501 line too long (85 > 79 characters)
main.py:17:5: F401 'os' imported but unused
```

Each warning follows the format: `file:line:column: CODE message`

### Configure `flake8`

Create a `.flake8` file in your project root to customize rules:

```ini
[flake8]
max-line-length = 88       # common when using Black formatter
exclude =
    .venv,
    __pycache__,
    migrations
ignore = E203, W503         # ignore specific codes
```

### Common Error Codes

| Code | Description |
|------|-------------|
| `E1xx` | Indentation errors |
| `E2xx` | Whitespace errors |
| `E3xx` | Blank line errors |
| `E5xx` | Line length errors |
| `F4xx` | Import errors |
| `W` prefix | Warnings |

>[!TIP]
>Integrate `flake8` into your editor (VS Code, PyCharm) or CI pipeline to catch issues before they reach production.
