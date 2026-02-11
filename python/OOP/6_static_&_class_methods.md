# About Static & Class Methods
## Instance Methods (Review)
Instance methods are the standard methods in Python classes. They:
- Take `self` as the first parameter
- Can access and modify instance attributes
- Can access class attributes through `self.__class__`
```python
class MyClass:
    def instance_method(self):
        return f"Called on {self}"
```

## Class Methods
### The @classmethod Decorator

Class methods are defined using the `@classmethod` decorator above the method definition.
```python
class MyClass:
    @classmethod
    def class_method(cls):
        return f"Called on {cls}"
```

### The cls Parameter
- First parameter is `cls` (by convention), representing the class itself
- Receives the class as an implicit first argument (not an instance)
- Can access class attributes but not instance attributes
- Can create and return new instances of the class
```python
class Person:
    species = "Homo sapiens"
    
    @classmethod
    def get_species(cls):
        return cls.species
    
    @classmethod
    def from_birth_year(cls, name, birth_year):
        age = 2024 - birth_year
        return cls(name, age)  # Creates new instance
```

### When to Use Class Methods
- **Alternative constructors**: Create instances from different input formats
- **Factory methods**: Build objects with preset configurations
- **Class-level operations**: Modify or access class attributes
- **Inheritance-aware logic**: When subclasses need different behavior
```python
class Date:
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day
    
    @classmethod
    def from_string(cls, date_string):
        year, month, day = map(int, date_string.split('-'))
        return cls(year, month, day)

# Usage
date = Date.from_string('2024-03-15')
```

## Static Methods

### The @staticmethod Decorator

Static methods are defined using the `@staticmethod` decorator.
```python
class MyClass:
    @staticmethod
    def static_method():
        return "No self or cls needed"
```

### Methods Without self or cls

- Don't receive `self` or `cls` automatically
- Cannot access instance or class attributes directly
- Behave like regular functions but belong to the class namespace
- Called on the class or instance (but don't receive either)
```python
class Math:
    @staticmethod
    def add(x, y):
        return x + y
    
    @staticmethod
    def is_even(n):
        return n % 2 == 0

# Usage
result = Math.add(5, 3)  # 8
```

### When to Use Static Methods

- **Utility functions**: Related to the class but don't need class/instance data
- **Namespace organization**: Group related functions within a class
- **No state needed**: Pure functions that logically belong to the class
- **Helper functions**: Internal operations that don't modify state
```python
class Validator:
    @staticmethod
    def is_valid_email(email):
        return '@' in email and '.' in email
    
    @staticmethod
    def is_valid_phone(phone):
        return len(phone) == 10 and phone.isdigit()
```

## Choosing the Right Method Type

| Method Type | First Parameter | Access To | Use When |
|-------------|----------------|-----------|----------|
| Instance | `self` | Instance & class data | Need instance-specific data |
| Class | `cls` | Class data, can create instances | Alternative constructors, factory methods |
| Static | None | No class/instance data | Utility functions related to class |
```python
class Pizza:
    base_price = 10
    
    def __init__(self, toppings):
        self.toppings = toppings
    
    # Instance method - needs instance data
    def get_price(self):
        return self.base_price + len(self.toppings) * 2
    
    # Class method - alternative constructor
    @classmethod
    def margherita(cls):
        return cls(['mozzarella', 'basil'])
    
    # Static method - utility function
    @staticmethod
    def is_valid_topping(topping):
        return isinstance(topping, str) and len(topping) > 0
```

**Key principle**: Use the least powerful method type that gets the job done. If you don't need `self`, use `@classmethod`. If you don't need `cls`, use `@staticmethod`.
