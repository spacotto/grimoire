# Attributes and Instance Variables
**Attributes** are **variables that belong to an object or class**. They store data associated with that object and define its state.

```python
class Dog:
    def __init__(self, name, age):
        self.name = name  # attribute
        self.age = age    # attribute

dog = Dog("Buddy", 3)
print(dog.name)  # Output: Buddy
```

On the other hand, **instance variables** are **attributes unique to each instance of a class**. Each object has its own copy of instance variables.

```python
class Car:
    def __init__(self, color, brand):
        self.color = color  # instance variable
        self.brand = brand  # instance variable

car1 = Car("red", "Toyota")
car2 = Car("blue", "Honda")

print(car1.color)  # Output: red
print(car2.color)  # Output: blue
```

## The self Parameter
`self` represents the instance of the class. It's used to access attributes and methods within the class.

- `self` is always the first parameter in instance methods
- It refers to the current object
- The name `self` is a convention (not a keyword), but should always be used

```python
class Person:
    def __init__(self, name):
        self.name = name  # self refers to the current instance
    
    def greet(self):
        return f"Hello, I'm {self.name}"

person = Person("Alice")
print(person.greet())  # Output: Hello, I'm Alice
```

## Accessing Attributes (`self.attribute`)
Access attributes using dot notation: `object.attribute` or `self.attribute` (within methods).

```python
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author
    
    def display(self):
        # Accessing within the class
        print(f"{self.title} by {self.author}")

book = Book("1984", "George Orwell")
book.display()  # Output: 1984 by George Orwell

# Accessing from outside the class
print(book.title)   # Output: 1984
print(book.author)  # Output: George Orwell
```

## Modifying Attributes
Attributes can be modified directly or through methods.

**Direct modification:**
```python
class Student:
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade

student = Student("John", 85)
student.grade = 90  # Direct modification
print(student.grade)  # Output: 90
```

**Modification through methods:**
```python
class BankAccount:
    def __init__(self, balance):
        self.balance = balance
    
    def deposit(self, amount):
        self.balance += amount
    
    def withdraw(self, amount):
        self.balance -= amount

account = BankAccount(1000)
account.deposit(500)
print(account.balance)  # Output: 1500
```

## Class Variables vs. Instance Variables
**Class variables** are shared across all instances. **Instance variables** are unique to each instance.

```python
class Employee:
    company = "TechCorp"  # Class variable (shared)
    
    def __init__(self, name, salary):
        self.name = name      # Instance variable (unique)
        self.salary = salary  # Instance variable (unique)

emp1 = Employee("Alice", 50000)
emp2 = Employee("Bob", 60000)

# Instance variables are different
print(emp1.name)    # Output: Alice
print(emp2.name)    # Output: Bob

# Class variable is shared
print(emp1.company)  # Output: TechCorp
print(emp2.company)  # Output: TechCorp

# Changing class variable affects all instances
Employee.company = "NewCorp"
print(emp1.company)  # Output: NewCorp
print(emp2.company)  # Output: NewCorp
```

**Key differences:**
| Class Variables | Instance Variables |
|----------------|-------------------|
| Defined inside class, outside methods | Defined inside `__init__` or other methods |
| Shared by all instances | Unique to each instance |
| Accessed via `ClassName.variable` | Accessed via `self.variable` |
| Modified with `ClassName.variable = value` | Modified with `self.variable = value` |

**Example showing both:**
```python
class Counter:
    total_count = 0  # Class variable
    
    def __init__(self, name):
        self.name = name           # Instance variable
        Counter.total_count += 1   # Increment shared counter
    
    def display(self):
        print(f"{self.name}: Total objects created = {Counter.total_count}")

c1 = Counter("First")
c2 = Counter("Second")
c3 = Counter("Third")

c1.display()  # Output: First: Total objects created = 3
c2.display()  # Output: Second: Total objects created = 3
```
