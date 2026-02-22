# Inheritance
Inheritance allows a class to acquire the properties and methods of another class. It promotes code reuse, logical hierarchy, and cleaner design by modelling real-world IS-A relationships.

## What is Inheritance?

Inheritance is a mechanism where a **child class** derives attributes and methods from a **parent class**. The child inherits all accessible members of the parent and can extend or override them.

```python
class Animal:
    def speak(self):
        print("Some sound")

class Dog(Animal):  # Dog inherits from Animal
    pass

d = Dog()
d.speak()  # Output: Some sound
```

## Parent Classes (Base Classes)

A **parent class** (also called base class or superclass) defines the common interface and behaviour shared by its children.

```python
class Vehicle:
    def __init__(self, brand):
        self.brand = brand

    def move(self):
        print(f"{self.brand} is moving")
```

## Child Classes (Derived Classes)

A **child class** (derived class or subclass) inherits from a parent class by passing it as an argument in the class definition.

```python
class Car(Vehicle):
    pass

car = Car("Toyota")
car.move()  # Output: Toyota is moving
```

## The `super()` Function

`super()` returns a proxy object that delegates method calls to the parent class. It's the standard way to access parent members from a child class.

```python
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)  # Call parent __init__
        self.breed = breed
```

## Calling Parent Constructors

Use `super().__init__()` inside the child's `__init__` to initialize inherited attributes properly.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

class Employee(Person):
    def __init__(self, name, age, company):
        super().__init__(name, age)
        self.company = company

emp = Employee("Alice", 30, "Acme")
print(emp.name, emp.age, emp.company)  # Alice 30 Acme
```

## Overriding Methods

A child class can **override** a parent method by redefining it with the same name.

```python
class Animal:
    def speak(self):
        print("Generic sound")

class Cat(Animal):
    def speak(self):       # Overrides parent method
        print("Meow")

Cat().speak()  # Output: Meow
```

## Extending Parent Functionality

Instead of fully replacing a method, call `super()` to run the parent's version and then add extra behavior.

```python
class Animal:
    def describe(self):
        print("I am an animal")

class Dog(Animal):
    def describe(self):
        super().describe()          # Run parent logic
        print("Specifically, a dog")

Dog().describe()
# I am an animal
# Specifically, a dog
```

## Adding New Methods in Child Classes

Child classes can define methods that don't exist in the parent.

```python
class Vehicle:
    def move(self):
        print("Moving")

class Boat(Vehicle):
    def sail(self):          # New method, exclusive to Boat
        print("Sailing on water")
```

## Adding New Attributes in Child Classes

New attributes are added in the child's `__init__`, alongside the call to `super().__init__()`.

```python
class Shape:
    def __init__(self, color):
        self.color = color

class Circle(Shape):
    def __init__(self, color, radius):
        super().__init__(color)
        self.radius = radius    # New attribute

c = Circle("red", 5)
print(c.color, c.radius)  # red 5
```

## Inheritance Hierarchies

Classes can form multi-level trees. Each level inherits from the one above it.

```python
Animal
 └── Mammal
      └── Dog
```

```python
class Animal: ...
class Mammal(Animal): ...
class Dog(Mammal): ...
```

## Multi-Level Inheritance

A class can inherit from a class that is itself a subclass, creating a chain.

```python
class A:
    def hello(self):
        print("Hello from A")

class B(A):
    pass

class C(B):   # C inherits from B which inherits from A
    pass

C().hello()   # Output: Hello from A
```

>[!NOTE]
>Python resolves method lookup using the **MRO (Method Resolution Order)**, which can be inspected with `ClassName.__mro__`.

## Code Reusability Through Inheritance

Inheritance avoids duplicating logic. Common behavior lives in the parent; specialized behavior lives in children.

```python
class Logger:
    def log(self, msg):
        print(f"[LOG]: {msg}")

class FileHandler(Logger):
    def save(self, data):
        self.log("Saving file...")   # Reused from Logger
        # ... file saving logic

class NetworkHandler(Logger):
    def send(self, data):
        self.log("Sending data...")  # Reused from Logger
        # ... network logic
```

## IS-A Relationships

Inheritance should model an **IS-A** relationship: the child *is a* type of the parent.

| Relationship | Correct? |
|---|---|
| `Dog` IS-A `Animal` | ✅ Yes — use inheritance |
| `Car` IS-A `Engine` | ❌ No — use composition |

```python
isinstance(Dog(), Animal)   # True — confirms IS-A
issubclass(Dog, Animal)     # True
```

>[!TIP]
>**Rule of thumb:** If you can't say "X is a Y" naturally, prefer **composition** over inheritance.
