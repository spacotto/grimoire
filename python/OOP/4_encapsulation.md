# About Encapsulation
Encapsulation is the practice of **bundling data** (attributes) and **methods** that operate on that data **within a single unit** (class), while **restricting direct access** to some components.

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.__salary = salary  # encapsulated data
    
    def get_salary(self):
        return self.__salary
```

Information hiding prevents external code from directly accessing or modifying internal state, reducing unintended side effects and bugs.
```python
class Database:
    def __init__(self):
        self.__connection = None  # hidden from outside
    
    def connect(self):
        self.__connection = "connected"
    
    def is_connected(self):
        return self.__connection is not None
```

| Access Level | Syntax    | Description                                          |
|--------------|-----------|------------------------------------------------------|
| Public       | `name`    | Accessible anywhere                                  |
| Protected    | `_name`   | Intended for internal use (convention only)          |
| Private      | `__name`  | Name mangling applied, harder to access from outside |

Python transforms `__attribute` to `_ClassName__attribute` to avoid accidental access.
```python
class Account:
    def __init__(self):
        self.__balance = 1000

acc = Account()
# acc.__balance              # AttributeError
print(acc._Account__balance)  # 1000 (possible but discouraged)
```

## Getter & Setter Methods
Getter methods provide controlled read access to private attributes.
```python
class Rectangle:
    def __init__(self, width, height):
        self.__width = width
        self.__height = height
    
    def get_width(self):
        return self.__width
    
    def get_height(self):
        return self.__height

rect = Rectangle(10, 5)
print(rect.get_width())  # 10
```

Setter methods provide controlled write access to private attributes.
```python
class Temperature:
    def __init__(self):
        self.__celsius = 0
    
    def set_celsius(self, value):
        self.__celsius = value
    
    def get_celsius(self):
        return self.__celsius

temp = Temperature()
temp.set_celsius(25)
print(temp.get_celsius())  # 25
```

## Data Validation in Setters

Setters can enforce rules and **prevent invalid data** and data corruption.
```python
class Person:
    def __init__(self, age):
        self.__age = 0
        self.set_age(age)
    
    def set_age(self, age):
        if age < 0 or age > 150:
            raise ValueError("Age must be between 0 and 150")
        self.__age = age
    
    def get_age(self):
        return self.__age

person = Person(25)
# person.set_age(-5)  # ValueError
```

## Puposes
Encapsulation provides:
- **Protection (Data Integrity)**: Prevents accidental modification of critical data, ensuring data remains valid and consistent throughout the object's lifetime.
- **Flexibility**: Internal implementation can change without breaking external code
- **Control**: Validation and business logic enforced at access points
- **Maintainability**: Clear interface between components
