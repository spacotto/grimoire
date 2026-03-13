# Advanced Inheritance Patterns

Inheritance allows classes to derive behavior from parent classes, enabling code reuse and polymorphic behavior. Advanced patterns extend basic inheritance with multiple inheritance, mixins, and strategic design choices. Understanding these patterns helps you build flexible, maintainable class hierarchies.

## Multiple Inheritance

Python supports multiple inheritance—a class can inherit from more than one parent class.
```python
class Flyable:
    def fly(self):
        return "Flying"

class Swimmable:
    def swim(self):
        return "Swimming"

class Duck(Flyable, Swimmable):
    pass

duck = Duck()
print(duck.fly())   # Flying
print(duck.swim())  # Swimming
```

>[!IMPORTANT]
>The child class inherits methods from all parent classes. Order matters when parents define the same method.

## Method Resolution Order (MRO) in Detail

MRO determines which method gets called when multiple parent classes define the same method. Python uses the C3 linearization algorithm.
```python
class A:
    def process(self):
        return "A"

class B(A):
    def process(self):
        return "B"

class C(A):
    def process(self):
        return "C"

class D(B, C):
    pass

print(D.mro())
# [<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>]

d = D()
print(d.process())  # "B" (follows MRO left to right)
```

>[!NOTE]
>MRO ensures each class appears only once and respects the order of inheritance. Check it with `ClassName.mro()` or `ClassName.__mro__`.

## Mixin Classes

Mixins are small classes that provide specific functionality without being full base classes. They're designed to be combined with other classes.

```python
class JSONMixin:
    def to_json(self):
        import json
        return json.dumps(self.__dict__)

class LogMixin:
    def log(self, message):
        print(f"[{self.__class__.__name__}] {message}")

class User(JSONMixin, LogMixin):
    def __init__(self, name, email):
        self.name = name
        self.email = email

user = User("Alice", "alice@example.com")
user.log("User created")
print(user.to_json())
```

>[!TIP]
>Mixins add behavior without creating deep inheritance hierarchies. Name them with a `Mixin` suffix for clarity.

## Diamond Problem

The diamond problem occurs when a class inherits from two classes that share a common ancestor.

```python
class Base:
    def action(self):
        print("Base action")

class Left(Base):
    def action(self):
        print("Left action")
        super().action()

class Right(Base):
    def action(self):
        print("Right action")
        super().action()

class Diamond(Left, Right):
    def action(self):
        print("Diamond action")
        super().action()

d = Diamond()
d.action()
# Diamond action
# Left action
# Right action
# Base action
```

>[!NOTE]
>Python's MRO ensures `Base.action()` is called only once, even though both `Left` and `Right` inherit from it.

## Cooperative Multiple Inheritance

Use `super()` to enable cooperative inheritance—each class in the chain calls the next, ensuring all parent methods execute.

```python
class Shape:
    def __init__(self, color):
        self.color = color
        super().__init__()

class Border:
    def __init__(self, thickness):
        self.thickness = thickness
        super().__init__()

class Rectangle(Shape, Border):
    def __init__(self, color, thickness, width, height):
        self.width = width
        self.height = height
        super().__init__(color=color, thickness=thickness)

rect = Rectangle("blue", 2, 10, 5)
```

>[!TIP]
>Always call `super().__init__()` and accept `**kwargs` to pass unused arguments up the chain. This pattern keeps initialization cooperative across multiple parents.

## Abstract vs. Concrete Methods

Abstract methods define interfaces without implementation. Concrete methods provide actual behavior.

```python
from abc import ABC, abstractmethod

class DataProcessor(ABC):
    @abstractmethod
    def process(self, data):
        """Must be implemented by subclasses"""
        pass
    
    def validate(self, data):
        """Concrete method - optional to override"""
        return data is not None

class CSVProcessor(DataProcessor):
    def process(self, data):
        return data.split(',')

# processor = DataProcessor()  # Error: Can't instantiate
csv = CSVProcessor()
```

>[!NOTE]
>Abstract methods enforce contracts. Concrete methods provide shared behavior that subclasses can inherit or override.

## Inheritance Hierarchies Design

Good hierarchies are shallow, focused, and follow the Liskov Substitution Principle—subclasses should be usable wherever parent classes are expected.

```python
# Good: Clear hierarchy, specific behaviors
class Vehicle:
    def start(self):
        pass

class Car(Vehicle):
    def start(self):
        return "Engine starting"

class ElectricCar(Car):
    def start(self):
        return "Battery powering on"
    
    def charge(self):
        return "Charging battery"
```

>[!TIP]
>Keep hierarchies shallow (2-3 levels max). Favor composition for complex behavior combinations.

## When to Use Composition Over Inheritance

Use composition when objects "have a" relationship rather than "is a" relationship.

```python
# Inheritance: "is a"
class Employee(Person):
    pass

# Composition: "has a"
class Car:
    def __init__(self):
        self.engine = Engine()
        self.wheels = [Wheel() for _ in range(4)]

class Engine:
    def start(self):
        return "Running"
```

**Choose composition when:**
- You need runtime flexibility (swap components)
- Behavior comes from multiple unrelated sources
- Inheritance would create awkward hierarchies
- You want to avoid tight coupling

**Choose inheritance when:**
- Clear "is a" relationship exists
- Shared behavior belongs to all subclasses
- Polymorphism is needed
- Building frameworks or libraries with extension points
