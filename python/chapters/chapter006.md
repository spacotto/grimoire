# String Operations
Strings in Python are immutable sequences of characters. Python provides a rich set of built-in tools for manipulating, formatting, and combining strings — making text processing concise and readable.

## String Concatenation
Use `+` to join strings, or `*` to repeat them.

```python
first = "Hello"
last = "World"
greeting = first + ", " + last + "!"  # "Hello, World!"
repeat = "ha" * 3                     # "hahaha"
```

>[!NOTE]
>Concatenating many strings in a loop is inefficient — prefer `"".join(list)` instead.

```python
words = ["Python", "is", "great"]
sentence = " ".join(words)  # "Python is great"
```

## String Methods
Strings come with many built-in methods. They always return a **new** string (strings are immutable).

```python
s = "  hello, world  "

s.strip()        # "hello, world"   → removes leading/trailing whitespace
s.replace("hello", "hi")  # "  hi, world  "
s.split(", ")    # ["  hello", "world  "]
s.find("world")  # 9               → index of first match, -1 if not found
s.startswith("  hello")  # True
s.endswith("  ")         # True
len(s)           # 16
```

## Case Manipulation

| Method          | Result              | Description                          |
|-----------------|---------------------|--------------------------------------|
| `capitalize()`  | `"Hello world"`     | First char upper, rest lower         |
| `upper()`       | `"HELLO WORLD"`     | All characters uppercase             |
| `lower()`       | `"hello world"`     | All characters lowercase             |
| `title()`       | `"Hello World"`     | First char of each word uppercase    |
| `swapcase()`    | `"hELLO wORLD"`     | Swaps case of every character        |

```python
text = "hello world"
text.capitalize()  # "Hello world"
text.upper()       # "HELLO WORLD"
text.title()       # "Hello World"
```

## String Formatting

**`%` formatting** (old-style):
```python
name = "Alice"
age = 30
"My name is %s and I am %d years old." % (name, age)
```

**`str.format()`** (modern):
```python
"My name is {} and I am {} years old.".format(name, age)
"Coords: x={x}, y={y}".format(x=10, y=20)
```

## F-strings (Formatted String Literals)
Introduced in Python 3.6. The most readable and preferred method.

```python
name = "Alice"
age = 30
print(f"My name is {name} and I am {age} years old.")
# "My name is Alice and I am 30 years old."
```

Supports expressions directly inside `{}`:
```python
x = 5
print(f"Square of {x} is {x**2}")   # "Square of 5 is 25"
print(f"Pi ≈ {3.14159:.2f}")        # "Pi ≈ 3.14"  → format specifier
print(f"{'hello':>10}")             # "     hello" → right-align in 10 chars
```

---

## Combining Different Data Types in Output
Python does **not** auto-convert types in concatenation — use `str()`, `f-strings`, or `format()`.

```python
score = 99
level = 3

# ❌ TypeError
# print("Score: " + score)

# ✅ Correct approaches
print("Score: " + str(score))              # explicit cast
print(f"Score: {score}, Level: {level}")   # f-string (preferred)
print("Score: {}, Level: {}".format(score, level))  # str.format()

# Mixed types in one line
pi = 3.14159
items = 7
print(f"Items: {items}, Pi: {pi:.3f}, Active: {True}")
# "Items: 7, Pi: 3.142, Active: True"
```
