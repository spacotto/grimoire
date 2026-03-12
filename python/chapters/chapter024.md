# Polymorphism Fundamentals

Polymorphism allows objects of different types to be treated through a common interface. This fundamental OOP concept enables writing flexible, extensible code where the same operation can behave differently depending on the object it acts upon.

## What is Polymorphism?

Polymorphism means "many forms." In programming, it refers to the ability of different objects to respond to the same method call in their own way. You write code against an interface or base type, and the actual behavior depends on the specific object at runtime.

>[!NOTE]
>**Key idea:** Write once, use with many types.

## Types of Polymorphism

Python supports several forms of polymorphism:

- **Subtype polymorphism** – Objects of derived classes can be used wherever the base class is expected
- **Duck typing** – If it walks like a duck and quacks like a duck, it's a duck (structural compatibility)
- **Operator overloading** – Built-in operators (`+`, `*`, etc.) behave differently for different types
- **Function overloading** – Same function name, different implementations (achieved through default arguments or conditional logic in Python)

## Subtype Polymorphism (Inheritance-based)

Subtype polymorphism occurs when derived classes override methods from a base class. Each subclass provides its own implementation while maintaining the same method signature.
```python
class Animal:
    def speak(self):
        raise NotImplementedError("Subclass must implement")

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

def make_sound(animal: Animal):
    print(animal.speak())

# Polymorphic behavior
make_sound(Dog())  # Woof!
make_sound(Cat())  # Meow!
```

## Duck Typing in Python

Python doesn't require explicit inheritance for polymorphism. Objects are compatible if they implement the required methods, regardless of their class hierarchy.
```python
class Duck:
    def quack(self):
        return "Quack!"

class Person:
    def quack(self):
        return "I'm imitating a duck!"

def make_it_quack(thing):
    # Works with any object that has a quack() method
    print(thing.quack())

make_it_quack(Duck())    # Quack!
make_it_quack(Person())  # I'm imitating a duck!
```

>[!NOTE]
>**Principle:** "Don't check types, just call methods." If the object supports the operation, it works.

## Polymorphic Behaviour

Polymorphic behaviour emerges when different objects respond to the same message in type-appropriate ways. The caller doesn't need to know the specific type—only that the object supports the required interface.
```python
shapes = [Circle(5), Rectangle(4, 6), Triangle(3, 4)]

total_area = sum(shape.area() for shape in shapes)
```

>[!NOTE]
>Each shape calculates its area differently, but the calling code treats them uniformly.

## Interface Consistency

Polymorphism requires consistent interfaces. All participating types must implement the same method signatures (name, parameters, return expectations).
```python
from abc import ABC, abstractmethod

class DataSource(ABC):
    @abstractmethod
    def read(self) -> str:
        """All data sources must implement read()"""
        pass

class FileSource(DataSource):
    def read(self) -> str:
        # Implementation for files
        pass

class APISource(DataSource):
    def read(self) -> str:
        # Implementation for API calls
        pass
```

>[!NOTE]
>Abstract base classes enforce this consistency at design time.

## Same Interface, Different Behaviour

The power of polymorphism lies in uniform interfaces with specialized implementations. You define what operations are available, and each type decides how to perform them.
```python
class Processor(ABC):
    @abstractmethod
    def process(self, data: str) -> str:
        pass

class UpperCaseProcessor(Processor):
    def process(self, data: str) -> str:
        return data.upper()

class ReverseProcessor(Processor):
    def process(self, data: str) -> str:
        return data[::-1]

# Client code doesn't care which processor
def apply_processing(processor: Processor, text: str):
    return processor.process(text)
```

## Benefits of Polymorphic Design

| Benefit | Explanation |
| :--- | :--- |
| Flexibility | Add new types without changing existing code. |
| Maintainability | Changes to one type don't affect others. |
| Testability | Easy to create mock objects for testing. |
| Readability | Code expresses intent at a high level, hiding implementation details. |
| Extensibility | New behaviours can be added by creating new classes, not modifying existing ones (Open/Closed Principle).|

**Example:**
```python
# Adding a new processor requires no changes to existing code
class EncryptProcessor(Processor):
    def process(self, data: str) -> str:
        return "".join(chr(ord(c) + 1) for c in data)

# Works immediately with all existing polymorphic functions
result = apply_processing(EncryptProcessor(), "hello")
```

>[!NOTE]
>Polymorphism is the foundation of flexible, scalable software design.
