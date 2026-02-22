# Classes and Objects
Python is an object-oriented language. Classes and objects are the foundation of OOP in Python — they let you model real-world entities, bundle data with behaviour, and write reusable, organised code.

## Classes as Blueprints

A **class** is a template that defines the structure and behavior of something. It specifies what data an entity holds (attributes) and what it can do (methods). The class itself holds no data — it just describes the pattern.

Think of a class like an architectural blueprint: it defines the design, but it isn't the building itself.

## Objects as Instances

An **object** is a concrete realisation of a class. When you create an object from a class, you get an **instance** — a real entity in memory with its own data, following the structure the class defined.

You can create many objects from one class, each independent of the others.

## The `class` Keyword

Use the `class` keyword to define a class in Python:

```python
class MyClass:
    pass
```

>[!NOTE]
>The body of a class contains attributes and methods. `pass` is used as a placeholder when the body is intentionally empty.

## Class Naming Conventions (PascalCase)

Class names in Python follow **PascalCase** (also called UpperCamelCase): each word starts with a capital letter, with no underscores.

```python
# Correct
class BankAccount:
    pass

class UserProfile:
    pass

# Incorrect
class bank_account:   # snake_case — used for functions/variables, not classes
    pass
```

>[!NOTE]
>This convention is defined in [PEP 8](https://peps.python.org/pep-0008/), Python's official style guide.

## Creating Your First Class

A minimal but meaningful class uses `__init__` — the **initialiser method** — to set up an object's attributes when it's created.

```python
class Dog:
    def __init__(self, name, breed):
        self.name = name      # instance attribute
        self.breed = breed    # instance attribute

    def bark(self):
        print(f"{self.name} says: Woof!")
```

- `__init__` runs automatically when a new object is created.
- `self` refers to the specific instance being created or used.
- Attributes set with `self.` belong to the instance.

## Instantiating Objects

To create an object (instantiate a class), call the class like a function:

```python
my_dog = Dog("Rex", "Labrador")
```

Python calls `__init__` automatically, passing `"Rex"` and `"Labrador"` as arguments. `self` is handled implicitly — you don't pass it manually.

Access attributes and call methods using dot notation:

```python
print(my_dog.name)   # Rex
print(my_dog.breed)  # Labrador
my_dog.bark()        # Rex says: Woof!
```

## Multiple Instances

Each object is independent. You can create as many instances as needed from the same class, each with its own data:

```python
dog1 = Dog("Rex", "Labrador")
dog2 = Dog("Bella", "Poodle")
dog3 = Dog("Max", "Bulldog")

dog1.bark()   # Rex says: Woof!
dog2.bark()   # Bella says: Woof!
dog3.bark()   # Max says: Woof!

print(dog1.name == dog2.name)  # False — separate instances, separate data
```

Changing one instance does not affect others:

```python
dog1.name = "Rocky"
print(dog1.name)   # Rocky
print(dog2.name)   # Bella — unchanged
```
