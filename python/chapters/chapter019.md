# Constructors and Initialisation
In Python, object initialisation is handled through a special method that runs automatically when a new instance is created. Unlike some languages, Python doesn't have a traditional constructor — instead, it separates object creation from object initialisation. Understanding this process is essential for writing clean, predictable class-based code.

## The `__init__()` Method

`__init__()` is the initialiser method called immediately after a new object is created. It sets up the object's initial state.

```python
class Dog:
    def __init__(self):
        print("A new dog was created.")

d = Dog()  # Output: A new dog was created.
```

## Constructor Parameters

Parameters passed during instantiation flow directly into `__init__()`. The first parameter is always `self` — a reference to the instance being created.

```python
class Dog:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed

d = Dog("Rex", "Labrador")
```

## Initialising Instance Variables

Inside `__init__()`, instance variables are assigned via `self`. Each instance gets its own copy.

```python
class Counter:
    def __init__(self):
        self.count = 0      # unique to each instance
        self.history = []   # each instance gets its own list
```

>[!WARNING]
>**Common mistake:** Defining mutable defaults (like lists or dicts) as class variables instead of instance variables causes them to be shared across all instances.
>```python
># Wrong
>class Bad:
>    items = []  # shared across ALL instances
>
># Correct
>class Good:
>    def __init__(self):
>        self.items = []  # separate per instance
>```

## Default Parameter Values in Constructors

Parameters can have defaults, making them optional at instantiation.

```python
class Dog:
    def __init__(self, name, breed="Unknown"):
        self.name = name
        self.breed = breed

d1 = Dog("Rex", "Labrador")
d2 = Dog("Buddy")  # breed defaults to "Unknown"
```

>[!WARNING]
>Never use mutable types (lists, dicts) as default parameter values — use `None` instead:
>```python
>def __init__(self, tags=None):
>    self.tags = tags if tags is not None else []
>```

## Object Creation Process

Python object creation is a two-step process:

1. **`__new__(cls)`** — allocates memory and returns a new instance (rarely overridden).
2. **`__init__(self)`** — receives the new instance and initialises its state.

```python
class MyClass:
    def __new__(cls):
        print("Creating instance")
        return super().__new__(cls)

    def __init__(self):
        print("Initialising instance")

obj = MyClass()
# Output:
# Creating instance
# Initialising instance
```

>[!IMPORTANT]
>For most use cases, you only need `__init__()`.

## Factory Pattern Basics

When object creation logic becomes complex, use class methods as named constructors (factory methods) instead of overloading `__init__()`.
```python
class Date:
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    @classmethod
    def from_string(cls, date_str):
        year, month, day = map(int, date_str.split("-"))
        return cls(year, month, day)

    @classmethod
    def today(cls):
        import datetime
        d = datetime.date.today()
        return cls(d.year, d.month, d.day)

d1 = Date(2024, 1, 15)
d2 = Date.from_string("2024-01-15")
d3 = Date.today()
```

>[!NOTE]
>Factory methods keep `__init__()` simple and provide clear, readable alternative construction paths.
