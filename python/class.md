# About Python Classes
A **class** is a **user-defined data type that bundles data and functionality together**.

## Basic Class Definition
```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def bark(self):
        return f"{self.name} says woof!"
```

>[!IMPORTANT]
>`__init__` is a special method called the **constructor**: it automatically runs when you create a new object.

>[!IMPORTANT]
>`self` is a **reference to the instance being created**: it must be the first parameter in instance methods.

## Creating Instances
```python
my_dog = Dog("Buddy", 3)
print(my_dog.name)  # Buddy
print(my_dog.bark())  # Buddy says woof!
```

## Class vs Instance Variables
```python
class Dog:
    species = "Canis familiaris"  # Class variable (shared by all instances)
    
    def __init__(self, name):
        self.name = name  # Instance variable (unique to each instance)
```

## Inheritance
```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return f"{self.name} barks"

class Cat(Animal):
    def speak(self):
        return f"{self.name} meows"
```

## Special Methods
```python
class Book:
    def __init__(self, title, pages):
        self.title = title
        self.pages = pages
    
    def __str__(self):
        return f"{self.title} ({self.pages} pages)"
    
    def __repr__(self):
        return f"Book('{self.title}', {self.pages})"
    
    def __len__(self):
        return self.pages
```

## Properties
```python
class Circle:
    def __init__(self, radius):
        self._radius = radius
    
    @property
    def radius(self):
        return self._radius
    
    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius must be positive")
        self._radius = value
    
    @property
    def area(self):
        return 3.14159 * self._radius ** 2
```

## Class Methods and Static Methods
```python
class MyClass:
    count = 0
    
    def __init__(self):
        MyClass.count += 1
    
    @classmethod
    def get_count(cls):
        return cls.count
    
    @staticmethod
    def is_valid(value):
        return value > 0
```

## Private Variables (Convention)
```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # Name mangling (becomes _BankAccount__balance)
    
    def deposit(self, amount):
        self.__balance += amount
    
    def get_balance(self):
        return self.__balance
```

## Multiple Inheritance
```python
class A:
    def method(self):
        return "A"

class B:
    def method(self):
        return "B"

class C(A, B):  # Method Resolution Order: C -> A -> B
    pass

obj = C()
print(obj.method())  # A
```

## Abstract Base Classes
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
    
    @abstractmethod
    def perimeter(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)
```

## Data Classes (Python 3.7+)
```python
from dataclasses import dataclass

@dataclass
class Point:
    x: float
    y: float
    
    def distance_from_origin(self):
        return (self.x**2 + self.y**2)**0.5

p = Point(3.0, 4.0)
print(p)  # Point(x=3.0, y=4.0)
```

## Common Patterns
### Singleton Pattern
```python
class Singleton:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

### Context Manager
```python
class FileManager:
    def __init__(self, filename):
        self.filename = filename
    
    def __enter__(self):
        self.file = open(self.filename, 'r')
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()

with FileManager('data.txt') as f:
    content = f.read()
```
