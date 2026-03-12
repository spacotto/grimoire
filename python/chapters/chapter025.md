# Method Overriding in Python

Method overriding lets a subclass redefine a method inherited from its parent class. It is the mechanism behind **subtype polymorphism**: the same method call behaves differently depending on the actual type of the object at runtime. Understanding method overriding — and when to use it — is essential for writing clean, extensible object-oriented code in Python.

## What is Method Overriding?

When a subclass defines a method with the **same name** as one in its parent class, the subclass version *overrides* the parent version. Any call to that method on a subclass instance will execute the subclass implementation, not the parent's.

```python
class Animal:
    def speak(self) -> str:
        return "..."

class Dog(Animal):
    def speak(self) -> str:          # overrides Animal.speak
        return "Woof!"

class Cat(Animal):
    def speak(self) -> str:          # overrides Animal.speak
        return "Meow!"

animals = [Dog(), Cat(), Animal()]
for animal in animals:
    print(animal.speak())
# Woof!
# Meow!
# ...
```

>[!NOTE]
>Python resolves which `speak` to call at **runtime**, based on the object's actual class.

## Overriding vs. Overloading

These two terms are often confused.

| | Overriding | Overloading |
|---|---|---|
| **Where** | Subclass redefines a parent method | Same class, multiple versions of the same method |
| **When resolved** | Runtime (dynamic dispatch) | Compile time (static dispatch) |
| **Python support** | ✅ Native | ❌ Not natively supported* |

>[!NOTE]
>Python does not support traditional overloading. You simulate it with default arguments, `*args`, or `functools.singledispatch`.

```python
# Simulating overloading in Python (not true overloading)
class Calculator:
    def add(self, a, b=0, c=0):
        return a + b + c
```

## Method Signatures and Compatibility

Python does not enforce strict signature compatibility when overriding, but good practice — and type checkers like `mypy` — expect the overriding method to be **substitutable** for the parent's.

This is the **Liskov Substitution Principle (LSP)**: any code that works with a parent instance should work with a subclass instance without knowing the difference.

```python
class Shape:
    def area(self) -> float:
        raise NotImplementedError

class Circle(Shape):
    def __init__(self, radius: float) -> None:
        self.radius = radius

    def area(self) -> float:          # same signature as parent
        return 3.14159 * self.radius ** 2
```

>[!TIP]
>Avoid adding required parameters to an overriding method — that breaks substitutability.

```python
# ❌ Bad: callers expecting Shape.area() cannot call this
class Broken(Shape):
    def area(self, scale: float) -> float:   # extra required arg
        return 0.0
```

## Overriding Parent Methods

To override, simply define a method with the same name in the subclass. No special keyword is needed.

```python
class Vehicle:
    def describe(self) -> str:
        return "I am a vehicle."

class ElectricCar(Vehicle):
    def describe(self) -> str:
        return "I am an electric car."

car = ElectricCar()
print(car.describe())   # I am an electric car.
```

Use `@override` (Python 3.12+) or a type-checker decorator to signal intent explicitly and catch typos early:

```python
from typing import override   # Python 3.12+

class ElectricCar(Vehicle):
    @override
    def describe(self) -> str:
        return "I am an electric car."
```

## Calling Parent Methods with `super()`

Sometimes you want to **extend** the parent's behaviour, not replace it entirely. Use `super()` to delegate to the parent implementation first.

```python
class Animal:
    def __init__(self, name: str) -> None:
        self.name = name

    def describe(self) -> str:
        return f"Name: {self.name}"

class Dog(Animal):
    def __init__(self, name: str, breed: str) -> None:
        super().__init__(name)          # parent handles name
        self.breed = breed

    def describe(self) -> str:
        base = super().describe()       # reuse parent output
        return f"{base}, Breed: {self.breed}"

dog = Dog("Rex", "Labrador")
print(dog.describe())   # Name: Rex, Breed: Labrador
```

>[!NOTE]
>`super()` is especially important in `__init__` to ensure the parent class is properly initialised before the subclass adds its own state.

## Behavioural Specialisation

Overriding is the primary tool for **specialising** behaviour. Each subclass adapts a shared interface to its own logic, while remaining interchangeable from the caller's perspective.

```python
class DataExporter:
    def export(self, data: list) -> str:
        raise NotImplementedError

class CSVExporter(DataExporter):
    def export(self, data: list) -> str:
        return "\n".join(",".join(str(v) for v in row) for row in data)

class JSONExporter(DataExporter):
    import json
    def export(self, data: list) -> str:
        import json
        return json.dumps(data)

def run_export(exporter: DataExporter, data: list) -> None:
    print(exporter.export(data))          # works for any exporter

run_export(CSVExporter(), [[1, 2], [3, 4]])
run_export(JSONExporter(), [[1, 2], [3, 4]])
```

>[!NOTE]
>The caller (`run_export`) never needs to know which concrete exporter it receives.

## When to Override Methods

Override a method when:

- **The default implementation does not fit the subclass** — the parent has a generic fallback and the subclass needs a concrete one.
- **You need to extend, not replace** — use `super()` to keep the parent logic and add to it.
- **You are implementing an abstract method** — the parent (or ABC) declares an interface; each subclass must provide the implementation.
- **You are adapting a hook or template method** — a design pattern where the parent defines a flow and subclasses fill in the steps.

>[!IMPORTANT]
>Do **not** override just to access an attribute, or when a simple composition would be cleaner. Overriding creates a tight coupling between parent and subclass.

## Method Resolution Order (MRO)

When Python looks up a method, it follows a deterministic order called the **MRO** (Method Resolution Order), computed using the **C3 linearisation** algorithm.

```python
class A:
    def hello(self) -> str:
        return "A"

class B(A):
    def hello(self) -> str:
        return "B"

class C(A):
    def hello(self) -> str:
        return "C"

class D(B, C):          # multiple inheritance
    pass

print(D().hello())      # B  — MRO: D → B → C → A
print(D.__mro__)
# (<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>)
```

Python searches each class in MRO order and uses the **first match** it finds. You can always inspect the MRO:

```python
print(D.__mro__)
# or
print(D.mro())
```

Key rules:
- A class always comes before its parents.
- If a class inherits from multiple parents, their order in the `class` declaration determines priority.
- `object` is always last.

>[!NOTE]
>Understanding the MRO is critical when working with multiple inheritance and `super()`, because `super()` does not simply call the direct parent — it calls the **next class in the MRO**.
