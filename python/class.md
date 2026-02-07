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

# Sources
- [Python Documentation: Classes](https://docs.python.org/3/tutorial/classes.html)
