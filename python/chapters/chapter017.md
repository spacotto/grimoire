# Attributes and Instance Variables
In Python, **attributes** are variables associated with an object or class. **Instance variables** are the most common type — unique to each object, defined using `self` inside a class. Understanding how attributes work is fundamental to writing clear, object-oriented Python.

## Understanding Attributes
An attribute is any variable bound to an object or class. Attributes store state and are accessed via dot notation (`object.attribute`).
```python
class Dog:
    species = "Canis lupus"  # class attribute

    def __init__(self, name):
        self.name = name      # instance attribute
```

## Instance Variables
Instance variables hold data specific to each object. They are created and assigned inside methods using `self`.
```python
class Car:
    def __init__(self, make, model):
        self.make = make   # unique to each Car instance
        self.model = model
```

Each instance maintains its own copy:
```python
car1 = Car("Toyota", "Corolla")
car2 = Car("Honda", "Civic")
# car1.make != car2.make
```

## The `self` Parameter
`self` refers to the current instance of the class. It is always the first parameter of instance methods and is passed automatically by Python.
```python
class Circle:
    def __init__(self, radius):
        self.radius = radius  # binds radius to this instance
```

>[!IMPORTANT]
>`self` is a convention, not a keyword — but always use it.

## Accessing Attributes (`self.attribute`)
Use dot notation to read an attribute from within the class or outside it.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Hi, I'm {self.name}")  # internal access

p = Person("Alice")
print(p.name)   # external access → "Alice"
p.greet()       # → "Hi, I'm Alice"
```

## Modifying Attributes
Attributes can be updated directly or through methods.

```python
class Counter:
    def __init__(self):
        self.count = 0

    def increment(self):
        self.count += 1

c = Counter()
c.increment()
c.count = 10  # direct modification
```

>[!TIP]
>Using methods is preferred — it encapsulates logic and maintains control over state changes.

## Class Variables vs. Instance Variables

| Feature         | Class Variable              | Instance Variable          |
|-----------------|-----------------------------|----------------------------|
| Defined in      | Class body (outside methods)| `__init__` or other methods|
| Shared across   | All instances               | Only the specific instance |
| Access          | `ClassName.var` or `self.var`| `self.var`                 |
| Override risk   | Can be shadowed by instance | N/A                        |

```python
class Employee:
    company = "Acme"       # class variable — shared

    def __init__(self, name):
        self.name = name   # instance variable — unique

e1 = Employee("Bob")
e2 = Employee("Sue")

print(e1.company)  # "Acme"
e1.company = "NewCorp"   # creates instance variable, shadows class var
print(e2.company)  # still "Acme"
```

>[!NOTE]
>Modifying a class variable via an instance creates a new instance variable — it does not change the class variable for other instances.
