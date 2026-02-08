# About Python Classes
A **class** is a **user-defined data type that bundles data and functionality together**. It is a **blueprint** for creating objects. It defines the structure and behaviour that objects of that type will have. 

>[!IMPORTANT]
>An **object** is a **specific instance** of a **class**. When you create an object from a class, you're instantiating it. Each object has **its own data** but shares the **same structure** defined by the class.

## How to Create a Class
Use the `class` keyword to define a class and use the initialiser Magic method:
```python
class Plant:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

>[!TIP]
>Class names should use `PascalCase`: capitalise the first letter of each word with no spaces or underscores.

>[!IMPORTANT]
>`__init__` is a special method called the **constructor**: it automatically runs when you create a new object.

>[!IMPORTANT]
>`self` is a **reference to the instance being created**: it must be the first parameter in instance methods.

## Instantiating Objects
Create an object by calling the class name like a function:
```python
rose = Plant("Rose", 3)
print(rose.name)  # Output: Rose
print(rose.age)   # Output: 3
```

## Multiple Instances
rose = Dog("Rose", 3)
iris = Dog("Iris", 8)

print(rose.name)  # Output: Rose
print(iris.name)  # Output: Iris

## Sources
- [Python Documentation: Classes](https://docs.python.org/3/tutorial/classes.html)
