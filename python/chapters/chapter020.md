# Encapsulation
Encapsulation is one of the four pillars of OOP. It bundles data (attributes) and methods that operate on that data into a single unit (class), while restricting direct access to some components. This prevents accidental modification and enforces controlled interaction with an object's internal state.

## Understanding Encapsulation
Encapsulation means **keeping the internal workings of an object hidden from the outside**. Instead of letting code directly read or modify attributes, you expose controlled interfaces (methods). This leads to cleaner, more maintainable, and more robust code.

## Data Protection and Information Hiding
Information hiding ensures the internal state is not directly exposed. External code should interact with objects through defined methods, not by accessing raw attributes. This separates *what* an object does from *how* it does it.

## Public vs. Private Attributes
- **Public**: Accessible from anywhere. No underscore prefix. `self.name`
- **Protected** *(convention)*: Prefixed with a single underscore. `self._name` — signals "internal use," but still accessible.
- **Private**: Prefixed with double underscore. `self.__name` — triggers name mangling, making external access harder.

```python
class Person:
    def __init__(self, name, age):
        self.name = name        # public
        self._nickname = "N/A"  # protected (convention)
        self.__age = age        # private
```

## Name Mangling with Double Underscores
When you prefix an attribute with `__`, Python internally renames it to `_ClassName__attribute`. This prevents accidental overriding in subclasses, but does **not** make it truly inaccessible.

```python
class Person:
    def __init__(self):
        self.__age = 30

p = Person()
# p.__age          → AttributeError
# p._Person__age   → 30 (still accessible, but discouraged)
```

## Getter Methods
A getter retrieves the value of a private attribute. Use the `@property` decorator for clean, Pythonic access.

```python
class Person:
    def __init__(self, age):
        self.__age = age

    @property
    def age(self):
        return self.__age

p = Person(30)
print(p.age)  # 30
```

## Setter Methods
A setter controls how a private attribute is modified. Use `@property_name.setter`.

```python
class Person:
    def __init__(self, age):
        self.__age = age

    @property
    def age(self):
        return self.__age

    @age.setter
    def age(self, value):
        self.__age = value

p = Person(30)
p.age = 31
```

## Data Validation in Setters
Setters are ideal for enforcing rules before assigning a value, preventing invalid state.

```python
@age.setter
def age(self, value):
    if not isinstance(value, int) or value < 0:
        raise ValueError("Age must be a non-negative integer.")
    self.__age = value
```

This ensures the object always holds valid data, regardless of what external code tries to assign.

## Why Encapsulation Matters
- **Maintainability**: Internal implementation can change without breaking external code.
- **Control**: You decide what can be read or modified, and how.
- **Debugging**: Fewer unexpected side effects since state changes go through defined paths.
- **Abstraction**: Users of a class don't need to know its internals.

## Protecting Data Integrity
Encapsulation enforces invariants — rules about what constitutes valid object state. By routing all modifications through setters with validation logic, you guarantee that:

- Invalid data is rejected early.
- The object is always in a consistent, predictable state.
- Business logic stays centralised inside the class, not scattered throughout the codebase.

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance

    @property
    def balance(self):
        return self.__balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit must be positive.")
        self.__balance += amount

    def withdraw(self, amount):
        if amount > self.__balance:
            raise ValueError("Insufficient funds.")
        self.__balance -= amount
```
