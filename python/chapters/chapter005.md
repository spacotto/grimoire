# Primitive Data Types
Python has several built-in primitive data types. This note covers the two most fundamental ones — **integers** and **strings** — along with how Python handles types and how to convert between them.

## Integers (`int`)

An integer is a whole number — positive, negative, or zero — with no decimal point.

```python
age = 25
temperature = -10
count = 0
```

Integers support standard arithmetic operations: `+`, `-`, `*`, `/`, `//` (floor division), `%` (modulo), `**` (exponentiation).

```python
10 + 3   # 13
10 / 3   # 3.3333... (returns float)
10 // 3  # 3 (floor division, returns int)
10 % 3   # 1 (remainder)
2 ** 8   # 256
```

>[!NOTE]
>Python integers have unlimited precision — no overflow.

## Strings (`str`)

A string is a sequence of characters enclosed in single or double quotes.

```python
name = "Alice"
greeting = 'Hello, World!'
```

**Common operations:**

```python
# Concatenation
"Hello" + " " + "World"  # "Hello World"

# Repetition
"ha" * 3  # "hahaha"

# Length
len("Python")  # 6

# Indexing (zero-based)
"Python"[0]   # "P"
"Python"[-1]  # "n"

# Slicing
"Python"[0:3]  # "Pyt"
```

**Useful string methods:**

```python
"hello".upper()        # "HELLO"
"HELLO".lower()        # "hello"
"  hi  ".strip()       # "hi"
"a,b,c".split(",")     # ["a", "b", "c"]
"Hello {}".format("Alice")  # "Hello Alice"
f"Hello {name}"        # f-string (preferred)
```

>[!NOTE]
>Strings are **immutable** — you cannot change a character in place; you create a new string instead.

## Understanding Type in Python

Python is **dynamically typed**: you don't declare types explicitly — Python infers them at runtime.

```python
x = 42        # int
y = "hello"   # str
```

Use `type()` to inspect a value's type:

```python
type(42)       # <class 'int'>
type("hello")  # <class 'str'>
```

Use `isinstance()` to check if a value is of a specific type:

```python
isinstance(42, int)    # True
isinstance("hi", str)  # True
isinstance(42, str)    # False
```

---

## Type Conversion

Python does **not** implicitly convert between unrelated types. Mixing incompatible types raises a `TypeError`:

```python
"Age: " + 25  # TypeError: can only concatenate str (not "int") to str
```

You must convert explicitly using built-in functions.

---

## Converting Strings to Integers: `int()`

Use `int()` to convert a string (or float) to an integer.

```python
int("42")    # 42
int("0")     # 0
int(3.9)     # 3 (truncates, does not round)
```

>[!WARNING]
>The string must represent a valid whole number — otherwise Python raises a `ValueError`.

```python
int("42abc")  # ValueError
int("3.14")   # ValueError (use float() first, then int())
```

**Safe conversion pattern:**

```python
value = input("Enter a number: ")
number = int(value)  # will raise ValueError if input is not a valid int
```

---

## Converting Numbers to Strings: `str()`

Use `str()` to convert any value to its string representation.

```python
str(42)     # "42"
str(3.14)   # "3.14"
str(True)   # "True"
str(None)   # "None"
```

This is commonly used when building strings that include numbers:

```python
age = 25
print("I am " + str(age) + " years old.")  # explicit conversion
print(f"I am {age} years old.")             # f-strings handle it automatically
```
