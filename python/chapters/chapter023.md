# Nested Classes and Composition
Python allows classes to be defined inside other classes (nested classes) and supports building complex objects from simpler ones (composition). Together, these tools help you write cleaner, better-organised, more modular code. This note covers when and how to use them.

## What are Nested Classes?

A nested class is a class defined inside the body of another class. It is scoped to the outer class and signals a tight logical relationship between the two.
```python
class Outer:
    class Inner:
        pass
```

>[!NOTE]
>The `Inner` class belongs to `Outer` — it's not a standalone entity in the module namespace.

## Defining Classes Within Classes
```python
class Car:
    class Engine:
        def __init__(self, horsepower):
            self.horsepower = horsepower

        def start(self):
            return "Engine started"

    def __init__(self, hp):
        self.engine = Car.Engine(hp)
```

>[!NOTE]
>The nested class is defined at class level, not inside `__init__`.

>[!NOTE]
>Instances of the nested class are typically created in the outer class's methods.

## When to Use Nested Classes

Use nested classes when:

- The inner class is **only meaningful in the context** of the outer class.
- You want to **hide implementation details** from the rest of the module.
- You're grouping **helper logic** that doesn't belong at module level.
- You need **namespace clarity** (e.g. `Tree.Node` vs a bare `Node`).

>[!TIP]
>Avoid them when the inner class could reasonably stand alone or be reused elsewhere.

## Accessing Nested Classes

From outside the outer class, use dot notation:
```python
engine = Car.Engine(200)
```

From inside the outer class, reference via the class name or `self.__class__`:
```python
class Car:
    class Engine:
        pass

    def build(self):
        return Car.Engine()  # explicit
```

>[!WARNING]
>Nested classes do **not** have automatic access to the outer class's instance (`self`). Pass it explicitly if needed.

## Helper Classes and Organisation

Nested classes are ideal for **private helpers** — supporting data structures, iterators, or nodes that exist solely to serve the outer class.
```python
class LinkedList:
    class Node:
        def __init__(self, value):
            self.value = value
            self.next = None

    def __init__(self):
        self.head = None
```

>[!NOTE]
>`Node` has no purpose outside `LinkedList`, so nesting it makes intent clear and keeps the module namespace clean.

## Namespace Management

Nesting controls visibility and signals ownership:

| Approach | Accessibility | Signal |
|---|---|---|
| Module-level class | Global | General-purpose |
| Nested class | Via outer class | Tightly coupled |

>[!TIP]
>Use `Tree.Node`, `Form.Field`, `Lexer.Token` patterns to communicate that the inner type is part of the outer type's contract.

## Composition vs. Inheritance

| | Composition | Inheritance |
|---|---|---|
| Relationship | HAS-A | IS-A |
| Coupling | Loose | Tight |
| Flexibility | High | Lower |
| Reuse | Via delegation | Via subclassing |

**Prefer composition** when you want to combine behaviours without creating deep class hierarchies. Use inheritance when there is a genuine subtype relationship.
```python
# Inheritance (IS-A)
class ElectricCar(Car): ...

# Composition (HAS-A)
class Car:
    def __init__(self):
        self.engine = Engine()
        self.gearbox = Gearbox()
```

## HAS-A Relationships

Composition models **HAS-A**: a `Car` has an `Engine`, a `Form` has `Fields`, an `Order` has `LineItems`.
```python
class Order:
    class LineItem:
        def __init__(self, product, qty):
            self.product = product
            self.qty = qty

    def __init__(self):
        self.items = []

    def add(self, product, qty):
        self.items.append(Order.LineItem(product, qty))
```

Key principles:
- The outer object **owns or contains** the inner object.
- The inner object's **lifetime** is typically tied to the outer.
- Behaviour is **delegated** to components rather than inherited.

This keeps responsibilities separated and makes each class easier to test and replace.
