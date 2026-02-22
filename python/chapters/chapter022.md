# Advanced Method Types
Python classes support three method types: **instance methods**, **class methods**, and **static methods**. Each serves a distinct purpose based on what data it needs to access — the instance, the class itself, or neither. Choosing the right type leads to cleaner, more intentional code.

## Instance Methods (Review)

The default method type. Receives `self` as the first parameter, giving access to the instance and its attributes.

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def bark(self):
        return f"{self.name} says woof!"

d = Dog("Rex")
d.bark()  # "Rex says woof!"
```

Use when the method needs to read or modify instance state.

---

## Class Methods

### The `@classmethod` Decorator

Defined with the `@classmethod` decorator. Python automatically passes the **class** as the first argument instead of the instance.

```python
class Dog:
    species = "Canis lupus familiaris"

    @classmethod
    def get_species(cls):
        return cls.species
```

### The `cls` Parameter

`cls` refers to the class itself (not an instance). By convention, it's named `cls`, just as instance methods use `self`. You can call other class methods or access class attributes via `cls`.

```python
class Dog:
    count = 0

    def __init__(self):
        Dog.count += 1

    @classmethod
    def how_many(cls):
        return cls.count
```

### When to Use Class Methods

- **Alternative constructors** — create instances from different input formats.
- **Accessing or modifying class-level state** shared across all instances.
- **Factory methods** that return an instance of the class.

```python
class Date:
    def __init__(self, year, month, day):
        self.year, self.month, self.day = year, month, day

    @classmethod
    def from_string(cls, date_str):
        year, month, day = map(int, date_str.split("-"))
        return cls(year, month, day)

d = Date.from_string("2024-06-15")
```

---

## Static Methods

### The `@staticmethod` Decorator

Defined with the `@staticmethod` decorator. Receives **no implicit first argument** — no `self`, no `cls`.

```python
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b
```

### Methods Without `self` or `cls`

Static methods are essentially plain functions namespaced inside a class. They cannot access or modify instance or class state unless explicitly passed as arguments.

```python
class Validator:
    @staticmethod
    def is_positive(n):
        return n > 0

Validator.is_positive(5)   # True
Validator.is_positive(-1)  # False
```

### When to Use Static Methods

- **Utility/helper functions** logically related to the class but independent of its state.
- When the method **doesn't need** instance or class data.
- To improve **code organisation** by grouping related functions within a class.

---

## Choosing the Right Method Type

| Needs access to...         | Use               | First param |
|----------------------------|-------------------|-------------|
| Instance state (`self`)    | Instance method   | `self`      |
| Class state / factory      | Class method      | `cls`       |
| Neither                    | Static method     | *(none)*    |

>[!TIP]
>**Quick rule of thumb:**
>- Modifying or reading instance data → `instance method`
>- Working with class-level data or creating instances → `@classmethod`
>- Pure logic with no state dependency → `@staticmethod`

```python
class Circle:
    pi = 3.14159

    def __init__(self, radius):
        self.radius = radius

    def area(self):                    # instance method
        return Circle.pi * self.radius ** 2

    @classmethod
    def unit_circle(cls):              # class method (factory)
        return cls(1)

    @staticmethod
    def is_valid_radius(r):            # static method (utility)
        return r > 0
```
