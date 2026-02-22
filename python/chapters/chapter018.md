# Methods
Methods are functions defined inside a class that operate on instances of that class. They enable objects to encapsulate behaviour alongside data, forming the core of object-oriented programming in Python.

## What are Methods?

A **method** is a callable attribute of a class. Unlike standalone functions, methods are bound to an object and have access to its data through the `self` parameter.

```python
class Dog:
    def bark(self):
        print("Woof!")
```

## Defining Instance Methods

Instance methods are defined inside a class using `def`, with `self` as the first parameter. They belong to individual instances of the class.

```python
class Circle:
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14159 * self.radius ** 2
```

## The `self` Parameter in Methods

`self` refers to the current instance of the class. Python passes it automatically when a method is called — you never pass it explicitly.

```python
class Person:
    def __init__(self, name):
        self.name = name  # 'self' binds 'name' to this instance

    def greet(self):
        print(f"Hi, I'm {self.name}")
```

>[!NOTE]
>`self` is not a keyword; it's a naming convention. You could technically use any name, but `self` is universally expected in Python code.

## Calling Methods on Objects

Methods are called using dot notation on an instance (`self.`).

```python
dog = Dog()
dog.bark()  # Python translates this to Dog.bark(dog)

c = Circle(5)
print(c.area())  # Output: 78.53975
```

## Methods vs. Functions

| | Function | Method |
|---|---|---|
| Defined | Outside a class | Inside a class |
| First param | Any / none | `self` (instance) |
| Called via | Name directly | Object + dot notation |
| Access to object data | No | Yes, via `self` |

```python
# Function
def add(a, b):
    return a + b

# Method
class Calculator:
    def add(self, a, b):
        return a + b
```

## Methods that Modify State

These methods change the internal data (attributes) of an object.
```python
class BankAccount:
    def __init__(self, balance=0):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount  # modifies state

    def withdraw(self, amount):
        if amount <= self.balance:
            self.balance -= amount
```

## Methods that Return Information

These methods read and return data from the object without changing it.
```python
class BankAccount:
    def get_balance(self):
        return self.balance  # returns info, no mutation

    def is_overdrawn(self):
        return self.balance < 0  # returns bool
```

>[!TIP]
>A clean design separates **mutating** methods from **querying** methods where possible (Command-Query Separation).
