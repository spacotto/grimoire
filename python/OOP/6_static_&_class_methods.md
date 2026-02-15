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

## Decorator Syntax vs Explicit Calls

### Syntactic Sugar

The `@decorator` syntax is **syntactic sugar**, a cleaner way to write the same operation.
```python
# Using decorator (syntactic sugar)
class MyClass:
    @classmethod
    def my_method(cls):
        return "class method"

# Equivalent explicit call
class MyClass:
    def my_method(cls):
        return "class method"
    my_method = classmethod(my_method)
```

### How It Works

Decorators are applied after the function is defined:
```python
# This decorator syntax...
@classmethod
def method(cls):
    pass

# ...is transformed into this
def method(cls):
    pass
method = classmethod(method)
```

The decorator takes the function as an argument and returns a modified version.

### Explicit Call Examples
```python
class Example:
    # Static method - explicit
    def util(x, y):
        return x + y
    util = staticmethod(util)
    
    # Class method - explicit
    def factory(cls):
        return cls()
    factory = classmethod(factory)
```

### Why Use Decorator Syntax?

**Readability**: The decorator syntax is clearer and more Pythonic.
```python
# Clear intent
@staticmethod
def calculate(x):
    return x * 2

# vs. less clear
def calculate(x):
    return x * 2
calculate = staticmethod(calculate)
```

**Less repetition**: You don't repeat the function name.

**Standard practice**: The `@decorator` syntax is the convention in Python code.

### When Explicit Calls Are Useful

- **Conditional decoration**: Apply decorators based on conditions
- **Dynamic decoration**: Choose decorators at runtime
- **Understanding**: Learning how decorators work under the hood
```python
class Config:
    USE_CACHE = True
    
    def get_data(self):
        return "data"
    
    # Conditionally make it a static method
    if not USE_CACHE:
        get_data = staticmethod(get_data)
```

**Bottom line**: Always use `@decorator` syntax in normal code. Use explicit calls only when you need dynamic behaviour.
