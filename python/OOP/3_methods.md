# About Python Methods
**Methods** are **functions that belong to objects**. They **define the behaviours** that objects of a class can perform. While functions exist independently, methods are **always associated with a class** and operate on **instances** of that class.

## Defining Instance Methods
Instance methods are defined inside a class using the `def` keyword. They must include `self` as the first parameter.

```python
class Dog:
    def bark(self):
        return "Woof!"
    
    def sit(self):
        print("The dog is sitting")
```

## The `self` Parameter in Methods
The `self` parameter is a reference to the current instance of the class. It allows methods to access and modify the object's attributes and call other methods.

```python
class Person:
    def __init__(self, name):
        self.name = name
    
    def greet(self):
        return f"Hello, I'm {self.name}"
```

>[!NOTE]
>`self` is not a keyword; it's a naming convention. You could technically use any name, but `self` is universally expected in Python code.

## Calling Methods on Objects
Methods are called using dot notation on an object instance. Python automatically passes the instance as the `self` argument.

```python
my_dog = Dog()
my_dog.bark()  # Returns "Woof!"
my_dog.sit()   # Prints "The dog is sitting"

person = Person("Alice")
person.greet()  # Returns "Hello, I'm Alice"
```

## Methods vs. Functions

| Aspect | Functions | Methods |
|--------|-----------|---------|
| Definition | Defined independently | Defined inside a class |
| First parameter | No special requirement | Must have `self` (for instance methods) |
| Calling | `function_name(args)` | `object.method_name(args)` |
| Access to object data | No direct access | Access via `self` |

```python
# Function
def calculate_area(length, width):
    return length * width

# Method
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width
    
    def calculate_area(self):
        return self.length * self.width
```

## Methods that Modify State
These methods change the object's attributes. They typically don't return a value (or return `None`).

```python
class BankAccount:
    def __init__(self, balance=0):
        self.balance = balance
    
    def deposit(self, amount):
        self.balance += amount
    
    def withdraw(self, amount):
        if amount <= self.balance:
            self.balance -= amount
        else:
            print("Insufficient funds")

account = BankAccount(100)
account.deposit(50)   # balance is now 150
account.withdraw(30)  # balance is now 120
```

## Methods that Return Information
These methods retrieve or calculate information about the object without modifying its state.

```python
class Circle:
    def __init__(self, radius):
        self.radius = radius
    
    def get_radius(self):
        return self.radius
    
    def calculate_area(self):
        return 3.14159 * self.radius ** 2
    
    def calculate_circumference(self):
        return 2 * 3.14159 * self.radius

circle = Circle(5)
print(circle.get_radius())              # 5
print(circle.calculate_area())          # 78.53975
print(circle.calculate_circumference()) # 31.4159
```

## Magic Methods
**Magic methods** (also called **dunder methods**) are special methods with **double underscores** (`__`) before and after their names. Python automatically calls these methods in response to certain operations. They allow you to **define how objects behave with built-in Python operations**.

>[!IMPORTANT]
>There are several **[magic methods](https://github.com/spacotto/grimoire/blob/main/python/_cheat_sheets/magic_methods.md)** ready to use, each one with a different purpose.

---

**Key Takeaways:**
- Methods are functions bound to objects
- `self` refers to the instance calling the method
- Methods can modify object state or return information
- Use methods to encapsulate behavior related to your objects
