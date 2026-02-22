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

## Decorator Syntax vs Explicit Calls

### Syntactic Sugar

The `@decorator` syntax is **syntactic sugar**, a cleaner way to write the same operation.
```python
# Using decorator (syntactic sugar)
class MyClass:
    @classmethod
    def my_method(cls):
        return "class method"

# Equivalent explicit call
class MyClass:
    def my_method(cls):
        return "class method"
    my_method = classmethod(my_method)
```

>[!NOTE]
>**Syntactic sugar**, a term coined by Peter J. Landin, refers to language syntax that makes code **more pleasant or readable without adding new functionality**. Like real sugar, it does not change the substance, only the experience.

### How It Works

Decorators are applied after the function is defined:
```python
# This decorator syntax...
@classmethod
def method(cls):
    pass

# ...is transformed into this
def method(cls):
    pass
method = classmethod(method)
```

The decorator takes the function as an argument and returns a modified version.

### Explicit Call Examples
```python
class Example:
    # Static method - explicit
    def util(x, y):
        return x + y
    util = staticmethod(util)
    
    # Class method - explicit
    def factory(cls):
        return cls()
    factory = classmethod(factory)
```

### Why Use Decorator Syntax?

**Readability**: The decorator syntax is clearer and more Pythonic.
```python
# Clear intent
@staticmethod
def calculate(x):
    return x * 2

# vs. less clear
def calculate(x):
    return x * 2
calculate = staticmethod(calculate)
```

**Less repetition**: You don't repeat the function name.

**Standard practice**: The `@decorator` syntax is the convention in Python code.

### When Explicit Calls Are Useful

- **Conditional decoration**: Apply decorators based on conditions
- **Dynamic decoration**: Choose decorators at runtime
- **Understanding**: Learning how decorators work under the hood
```python
class Config:
    USE_CACHE = True
    
    def get_data(self):
        return "data"
    
    # Conditionally make it a static method
    if not USE_CACHE:
        get_data = staticmethod(get_data)
```

**Bottom line**: Always use `@decorator` syntax in normal code. Use explicit calls only when you need dynamic behaviour.
