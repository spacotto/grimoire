# Abstract Factory Pattern

The Abstract Factory pattern is a **creational design pattern** that provides an interface for creating *families of related objects* without specifying their concrete classes. It builds on the Factory Method pattern by grouping multiple factories under a common interface, ensuring that products from the same family are always used together.

## What is the Abstract Factory Pattern?

The Abstract Factory pattern solves a recurring problem: how to create groups of objects that are designed to work together, while keeping your code independent of their concrete implementations.

Think of it as a **factory of factories**. Instead of calling a constructor directly, your code asks an abstract factory to create the objects it needs. Depending on which concrete factory is in use, you get a different but internally consistent set of objects — the *product family*.

**Core components:**
- `AbstractFactory` — declares creation methods for each product type
- `ConcreteFactory` — implements creation methods for a specific product family
- `AbstractProduct` — declares the interface for a type of product
- `ConcreteProduct` — a specific product created by a matching concrete factory
- `Client` — uses only abstract interfaces; never instantiates products directly

```python
from abc import ABC, abstractmethod

# Abstract products
class Button(ABC):
    @abstractmethod
    def render(self) -> str: ...

class Checkbox(ABC):
    @abstractmethod
    def render(self) -> str: ...

# Abstract factory
class UIFactory(ABC):
    @abstractmethod
    def create_button(self) -> Button: ...

    @abstractmethod
    def create_checkbox(self) -> Checkbox: ...
```

## Factory Method vs. Abstract Factory

Both patterns delegate object creation — but they differ in **scope** and **intent**.

| | Factory Method | Abstract Factory |
|---|---|---|
| **Creates** | One product type | A family of related products |
| **Structure** | Single method override | Multiple methods per factory |
| **Variation axis** | One product varies | Whole product family varies |
| **Coupling** | Subclasses choose product | Factory object chooses family |

- **Factory Method** is a single method (often in a base class) that subclasses override to produce one kind of object.
- **Abstract Factory** is an object with *multiple* creation methods, where you swap the entire factory to switch product families.

```python
# Factory Method — one product, one method to override
class Dialog(ABC):
    @abstractmethod
    def create_button(self) -> Button: ...  # subclasses decide which Button

# Abstract Factory — multiple products, swap the whole factory
class UIFactory(ABC):
    @abstractmethod
    def create_button(self) -> Button: ...
    @abstractmethod
    def create_checkbox(self) -> Checkbox: ...
```

>[!TIP]
>Use Factory Method when variation involves a single product type. Use Abstract Factory when variation involves a whole family.

## Defining Abstract Factory Interfaces

Abstract factories are best defined with Python's `abc` module. Each creation method corresponds to one product type in the family.

```python
from abc import ABC, abstractmethod

class UIFactory(ABC):
    """Creates a family of related UI components."""

    @abstractmethod
    def create_button(self) -> "Button": ...

    @abstractmethod
    def create_checkbox(self) -> "Checkbox": ...

    @abstractmethod
    def create_text_input(self) -> "TextInput": ...
```

**Design guidelines:**
- Keep the interface focused — one creation method per product type
- Return abstract product types, not concrete ones
- Avoid adding logic to the abstract factory; keep it declarative
- Name methods consistently: `create_<product>()` is idiomatic

## Concrete Factory Implementations

Each concrete factory implements the abstract interface for a specific product family. Swapping factories switches the entire family at once.

```python
# Concrete products — Windows family
class WindowsButton(Button):
    def render(self) -> str:
        return "[Windows Button]"

class WindowsCheckbox(Checkbox):
    def render(self) -> str:
        return "[Windows Checkbox]"

# Concrete products — macOS family
class MacButton(Button):
    def render(self) -> str:
        return "(Mac Button)"

class MacCheckbox(Checkbox):
    def render(self) -> str:
        return "(Mac Checkbox)"

# Concrete factories
class WindowsUIFactory(UIFactory):
    def create_button(self) -> Button:
        return WindowsButton()

    def create_checkbox(self) -> Checkbox:
        return WindowsCheckbox()

class MacUIFactory(UIFactory):
    def create_button(self) -> Button:
        return MacButton()

    def create_checkbox(self) -> Checkbox:
        return MacCheckbox()
```

>[!NOTE]
>Each factory is a self-contained unit. Adding a new platform means adding a new factory class and new product classes — existing code is untouched.

## Product Families

A **product family** is a group of objects designed to work together. Abstract Factory enforces consistency: if you use `WindowsUIFactory`, all your components will be Windows-style.

```python
# Each factory produces a coherent family
class LightThemeFactory(UIFactory):
    def create_button(self) -> Button:
        return LightButton()       # light-styled

    def create_checkbox(self) -> Checkbox:
        return LightCheckbox()     # light-styled

class DarkThemeFactory(UIFactory):
    def create_button(self) -> Button:
        return DarkButton()        # dark-styled

    def create_checkbox(self) -> Checkbox:
        return DarkCheckbox()      # dark-styled
```

>[!NOTE]
>**Key benefit:** the client can never accidentally mix a `DarkButton` with a `LightCheckbox` — the factory guarantees consistency across the family.

## Creating Related Objects

The client creates objects exclusively through the factory. It stores the factory reference and calls its methods — never calling concrete constructors directly.

```python
class Application:
    def __init__(self, factory: UIFactory) -> None:
        self._factory = factory
        self._button = factory.create_button()
        self._checkbox = factory.create_checkbox()

    def render(self) -> None:
        print(self._button.render())
        print(self._checkbox.render())

# Usage — inject the factory at construction time
app = Application(WindowsUIFactory())
app.render()
# [Windows Button]
# [Windows Checkbox]

app = Application(MacUIFactory())
app.render()
# (Mac Button)
# (Mac Checkbox)
```

>[!NOTE]
>The `Application` class is completely decoupled from concrete products. Switching families requires only changing which factory is injected.

## Factory Registration and Discovery

For extensible systems, use a **registry** to map keys to factory classes. This avoids hardcoded `if/elif` chains and makes it easy to add new factories without modifying existing code.

```python
from typing import Type

_registry: dict[str, Type[UIFactory]] = {}

def register_factory(name: str, factory_class: Type[UIFactory]) -> None:
    _registry[name] = factory_class

def get_factory(name: str) -> UIFactory:
    factory_class = _registry.get(name)
    if factory_class is None:
        raise ValueError(f"No factory registered for '{name}'")
    return factory_class()

# Register factories
register_factory("windows", WindowsUIFactory)
register_factory("mac", MacUIFactory)

# Discover and use at runtime
factory = get_factory("mac")
app = Application(factory)
app.render()
```

For plugin-based systems, registration can happen at import time using decorators:

```python
def factory_for(name: str):
    """Class decorator that auto-registers a factory."""
    def decorator(cls: Type[UIFactory]) -> Type[UIFactory]:
        register_factory(name, cls)
        return cls
    return decorator

@factory_for("windows")
class WindowsUIFactory(UIFactory):
    ...
```

## When to Use Abstract Factory

Use Abstract Factory when:

- Your code needs to work with **multiple families** of related products (e.g., cross-platform UI, multiple database backends, themed components)
- You need to **enforce consistency** — products within a family must be used together
- You want to **hide concrete implementations** from client code
- You anticipate needing to **add new product families** without touching existing client code
- You're working with **dependency injection** and want to inject entire families at once

Avoid Abstract Factory when:

- You only have **one product type** — a simple Factory Method is sufficient
- Your product families are unlikely to **grow or change** — the abstraction may be overkill
- You need **fine-grained control** over individual product variants — the pattern locks in whole families

## Benefits and Trade-offs

### Benefits

- **Consistency** — the factory guarantees all created products belong to the same family; no accidental mismatches.
- **Open/Closed Principle** — new product families are added by creating new factory and product classes, not by modifying existing ones.
- **Single Responsibility** — product creation logic is centralized in factory classes, keeping client code clean.
- **Dependency Inversion** — clients depend on abstract interfaces, not concrete implementations; makes testing and swapping easier.
- **Encapsulation** — clients never need to know which concrete classes are in use.

### Trade-offs

- **Complexity** — introduces many new classes and interfaces. For simple use cases, this overhead may not be justified.
- **Rigidity around products** — adding a *new product type* to the family requires updating *every* concrete factory. This can be painful if factories are spread across a large codebase.
- **Harder to extend per-product** — since the factory controls all creation, it's less flexible when you want to customize one product independently from the family.
- **Indirection** — following the code path from factory call to concrete product requires more navigation than direct instantiation.

>[!TIP]
>**Rule of thumb:** If you find yourself writing `if platform == "windows": ... elif platform == "mac": ...` in multiple places across your codebase, Abstract Factory is likely the right cure.
