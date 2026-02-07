# About Python Classes
A **class** is a **user-defined data type that bundles data and functionality together**.

## Basic Class Definition
```python
class Plant:
    def __init__(self, name: str, height: int, age: int) -> None:
        self.name: str = name
        self.height: int = height
        self.age: int = age
    
    def grow(self):
        return f"{self.name} is growing!"
```

>[!IMPORTANT]
>`__init__` is a special method called the **constructor**: it automatically runs when you create a new object.

>[!IMPORTANT]
>`self` is a **reference to the instance being created**: it must be the first parameter in instance methods.

## PEP 8 Style Guidelines
Python follows the PEP 8 style guide for naming conventions within classes, which helps create consistent and readable code across the Python community. 
- **Class names** should use `CapitalizedWords` (also called **PascalCase** or **CapWords**), where each word starts with a capital letter, and there are no underscores—for example, `Dog`, `BankAccount`, or `HttpResponse`.
- **Method names** and **instance variables** should use `lowercase_with_underscores` (**snake_case**), such as `get_balance()`, `calculate_total()`, or `user_name`.
- **Constants** (class-level variables that shouldn't change) should be in `ALL_CAPS_WITH_UNDERSCORES`, like `MAX_SIZE` or `DEFAULT_TIMEOUT`.
- Prefix **internal/private attributes** with a single underscore like `_internal_method()` or `_private_data`, and use double underscores `__name_mangled` when you need name mangling to prevent conflicts in inheritance.
- 
Avoid using double underscores at both the beginning and end (like `__special__`) unless you're implementing Python's special methods like `__init__`, `__str__`, or `__len__`, as these are reserved for Python's internal use. 
```
class Plant:  # CapitalizedWords for class names
    MAX_HEIGHT = 300  # ALL_CAPS for constants (cm)
    MIN_WATER_LEVEL = 20  # ALL_CAPS for constants (%)
    
    def __init__(self, species_name, height):  # lowercase_with_underscores for methods
        self.species_name = species_name  # lowercase_with_underscores for attributes
        self.height = height  # lowercase_with_underscores for attributes
        self._water_level = 100  # Single underscore for internal use
        self.__growth_rate = 0.5  # Double underscore for name mangling
    
    def get_water_level(self):  # lowercase_with_underscores for method names
        return self._water_level
    
    def water_plant(self, amount):  # lowercase_with_underscores for method names
        self._water_level = min(100, self._water_level + amount)
    
    def _calculate_growth(self):  # Single underscore for internal method
        if self._water_level > self.MIN_WATER_LEVEL:
            return self.__growth_rate * self._water_level / 100
        return 0
    
    def grow(self):  # Public method
        growth = self._calculate_growth()
        self.height += growth
        self._water_level -= 5
```

# Sources
- [Python Documentation: Classes](https://docs.python.org/3/tutorial/classes.html)
- [PEP 8](https://peps.python.org/pep-0008)
