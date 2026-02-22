# Introduction to OOP in Python 3
Object-Oriented Programming (OOP) is a programming paradigm that organizes code around **objects** — entities that bundle data and behavior together. Python supports OOP natively and makes it straightforward to apply. This guide covers the core concepts, how OOP compares to procedural programming, and when to use it.

## What is Object-Oriented Programming?
OOP structures programs using **classes** and **objects**.

- A **class** is a blueprint that defines attributes (data) and methods (behaviou).
- An **object** is an instance of a class — a concrete entity created from that blueprint.

```python
class Dog:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed

    def bark(self):
        return f"{self.name} says: Woof!"

my_dog = Dog("Rex", "Labrador")
print(my_dog.bark())  # Rex says: Woof!
```

The four core pillars of OOP are:
1. **Encapsulation** – bundling data and methods; restricting direct access.
2. **Abstraction** – hiding complexity, exposing only what's needed.
3. **Inheritance** – a class can inherit attributes/methods from another class.
4. **Polymorphism** – different objects can share the same interface but behave differently.

## Objects in the Real World
OOP mirrors how we naturally think about things. A **Car** has attributes (colour, speed) and behaviours (accelerate, brake). A **BankAccount** has a balance and can deposit or withdraw funds.

This mapping makes it easier to model real-world problems in code:

| Real World | OOP Equivalent |
|------------|----------------|
| Blueprint  | Class          |
| Thing      | Object         |
| Property   | Attribute      |
| Action     | Method         |

## OOP vs. Procedural Programming

| Aspect        | Procedural                        | OOP                                  |
|---------------|-----------------------------------|--------------------------------------|
| Focus         | Functions and logic flow          | Objects and their interactions       |
| Data          | Passed between functions          | Encapsulated within objects          |
| Reusability   | Function reuse                    | Class inheritance and composition    |
| Scalability   | Can get complex as code grows     | Better suited for large codebases    |
| Example use   | Scripts, data pipelines           | GUIs, game engines, large apps       |

>[!NOTE]
>Procedural code tells the computer *how* to do things step by step. OOP defines *what things are* and *what they can do*.

## Benefits of OOP
- **Modularity** – code is organised into self-contained classes, easier to manage.
- **Reusability** – inherit and extend existing classes instead of rewriting logic.
- **Maintainability** – isolate changes to a class without affecting the rest of the codebase.
- **Readability** – code maps to real-world concepts, making it easier to understand.
- **Testability** – objects can be tested in isolation.

## When to Use OOP
OOP is a strong choice when:

- The problem involves **multiple entities** with shared structure (e.g., users, products, vehicles).
- The codebase is **large and collaborative** — classes provide clear boundaries.
- You need **extensibility** — new types can be added via subclassing without touching existing code.
- You're building **reusable libraries or frameworks**.

OOP may be overkill when:
- The task is a **simple script** with a single purpose.
- There's **no meaningful state** to manage.
- A **functional or procedural approach** is cleaner and more readable.

>[!TIP]
>**Rule of thumb:** If you're modelling entities with both data and behaviour, OOP is likely the right tool.
