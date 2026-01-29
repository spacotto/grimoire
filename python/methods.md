# About Python Methods
**Methods** are **functions** that belong to objects. They are called using dot notation and can access/modify the object's data.
```python
# Method call syntax
object.method(arguments)
```

## Instance Methods

Most common type. Operate on instance data and take `self` as first parameter.
```python
class Dog:
    def __init__(self, name):
        self.name = name
    
    def bark(self):
        return f"{self.name} says woof!"

dog = Dog("Max")
dog.bark()  # "Max says woof!"
```

## Class Methods

Operate on class itself, not instances. Use `@classmethod` decorator and take `cls` as first parameter.
```python
class Dog:
    species = "Canis familiaris"
    
    @classmethod
    def get_species(cls):
        return cls.species
    
    @classmethod
    def create_puppy(cls, name):
        return cls(name)  # Alternative constructor

Dog.get_species()  # "Canis familiaris"
puppy = Dog.create_puppy("Buddy")
```

## Static Methods

Don't access instance or class data. Use `@staticmethod` decorator. No `self` or `cls` parameter.
```python
class MathUtils:
    @staticmethod
    def add(x, y):
        return x + y

MathUtils.add(5, 3)  # 8
```

## Magic Methods (Dunder Methods)

Special methods with double underscores. Define how objects behave with built-in operations.
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return f"Point({self.x}, {self.y})"
    
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
    
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

p1 = Point(1, 2)
p2 = Point(3, 4)
print(p1)       # Point(1, 2)
p3 = p1 + p2    # Point(4, 6)
p1 == p2        # False
```

### Common Magic Methods

| Method | Purpose | Example |
|--------|---------|---------|
| `__init__` | Constructor | `obj = MyClass()` |
| `__str__` | String representation | `str(obj)`, `print(obj)` |
| `__repr__` | Developer representation | `repr(obj)` |
| `__len__` | Length | `len(obj)` |
| `__getitem__` | Index access | `obj[key]` |
| `__setitem__` | Index assignment | `obj[key] = value` |
| `__call__` | Make object callable | `obj()` |
| `__eq__` | Equality | `obj1 == obj2` |
| `__lt__` | Less than | `obj1 < obj2` |
| `__add__` | Addition | `obj1 + obj2` |

## Property Methods

Use `@property` to create managed attributes with getter/setter logic.
```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius
    
    @property
    def celsius(self):
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Below absolute zero")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        return (self._celsius * 9/5) + 32

temp = Temperature(25)
print(temp.celsius)      # 25
print(temp.fahrenheit)   # 77.0
temp.celsius = 30        # Uses setter
```

## Method Types Summary

| Type | Decorator | First Parameter | Access To |
|------|-----------|-----------------|-----------|
| Instance | None | `self` | Instance data |
| Class | `@classmethod` | `cls` | Class data |
| Static | `@staticmethod` | None | Nothing |
| Property | `@property` | `self` | Instance data |

## Best Practices

- Use instance methods for operations on object data
- Use class methods for alternative constructors or class-level operations
- Use static methods for utility functions related to the class
- Use properties for computed attributes or validated access
- Keep methods focused and single-purpose
- Name methods with verbs (actions)
