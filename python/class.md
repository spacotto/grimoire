# About Python Classes
A **class** is a **user-defined data type that bundles data and functionality together**.

## Basic Class Definition
```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def bark(self):
        return f"{self.name} says woof!"
```

>[!IMPORTANT]
>`__init__` is a special method called the **constructor**: it automatically runs when you create a new object.

>[!IMPORTANT]
>`self` is a **reference to the instance being created**: it must be the first parameter in instance methods.

# Sources
- [Python Documentation: Classes](https://docs.python.org/3/tutorial/classes.html)
