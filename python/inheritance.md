# About Inheritance
Inheritance allows a class to **inherit attributes and methods from another class**. It fosters code reuse and establishes relationships between classes.
```python
class Animal:
    def speak(self):
        return "Some sound"

class Dog(Animal):  # Dog inherits from Animal
    pass

dog = Dog()
print(dog.speak())  # "Some sound"
```

## Parent Classes (Base Classes)
The parent class (or base class) is the class being inherited from. It contains common attributes and methods.
```python
class Vehicle:
    def __init__(self, brand):
        self.brand = brand
    
    def start(self):
        return f"{self.brand} is starting"
```

## Child Classes (Derived Classes)
The child class (or derived class) inherits from the parent class. Define it using parentheses.
```python
class Car(Vehicle):  # Car is the child, Vehicle is the parent
    pass

my_car = Car("Toyota")
print(my_car.start())  # "Toyota is starting"
```

## The super() Function
`super()` gives access to methods in the parent class. Commonly used to call parent methods from child classes.
```python
class Parent:
    def greet(self):
        return "Hello from Parent"

class Child(Parent):
    def greet(self):
        parent_greeting = super().greet()
        return f"{parent_greeting} and Child"

c = Child()
print(c.greet())  # "Hello from Parent and Child"
```

## Calling Parent Constructors
Use `super().__init__()` to call the parent's constructor and initialize inherited attributes.
```python
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)  # Call parent constructor
        self.breed = breed

dog = Dog("Buddy", "Golden Retriever")
print(dog.name)   # "Buddy"
print(dog.breed)  # "Golden Retriever"
```

## Overriding Methods
Child classes can replace parent methods by defining a method with the same name.
```python
class Bird:
    def move(self):
        return "Flying"

class Penguin(Bird):
    def move(self):  # Override
        return "Swimming"

p = Penguin()
print(p.move())  # "Swimming"
```

## Extending Parent Functionality
Use `super()` to call the parent method, then add additional behaviour.
```python
class Employee:
    def work(self):
        return "Working"

class Manager(Employee):
    def work(self):
        base_work = super().work()
        return f"{base_work} and managing team"

m = Manager()
print(m.work())  # "Working and managing team"
```

## Adding New Methods in Child Classes
Child classes can have methods that don't exist in the parent.
```python
class Animal:
    def eat(self):
        return "Eating"

class Cat(Animal):
    def meow(self):  # New method
        return "Meow!"

cat = Cat()
print(cat.eat())   # "Eating" (inherited)
print(cat.meow())  # "Meow!" (new)
```

## Adding New Attributes in Child Classes
Child classes can define their own attributes in addition to inherited ones.
```python
class Person:
    def __init__(self, name):
        self.name = name

class Student(Person):
    def __init__(self, name, student_id):
        super().__init__(name)
        self.student_id = student_id  # New attribute

s = Student("Alice", "S12345")
print(s.name)        # "Alice"
print(s.student_id)  # "S12345"
```

## Inheritance Hierarchies
Multiple levels of inheritance create a hierarchy. A child class can become a parent to another class.
```python
class LivingThing:
    def breathe(self):
        return "Breathing"

class Animal(LivingThing):
    def move(self):
        return "Moving"

class Dog(Animal):
    def bark(self):
        return "Barking"

dog = Dog()
print(dog.breathe())  # "Breathing" (from LivingThing)
print(dog.move())     # "Moving" (from Animal)
print(dog.bark())     # "Barking" (from Dog)
```

## Multi-Level Inheritance
Inheritance can span multiple levels, where each class inherits from the one above it.
```python
class A:
    def method_a(self):
        return "Method A"

class B(A):
    def method_b(self):
        return "Method B"

class C(B):
    def method_c(self):
        return "Method C"

obj = C()
print(obj.method_a())  # Accessible through inheritance chain
print(obj.method_b())
print(obj.method_c())
```

## Code Reusability Through Inheritance
Inheritance eliminates duplicate code by allowing common functionality to be defined once in a parent class.
```python
class Shape:
    def __init__(self, color):
        self.color = color
    
    def describe(self):
        return f"A {self.color} shape"

class Circle(Shape):
    pass

class Square(Shape):
    pass

c = Circle("red")
s = Square("blue")
print(c.describe())  # "A red shape"
print(s.describe())  # "A blue shape"
```

## `IS-A` Relationship
Inheritance represents an `IS-A` relationship. A child class `IS-A` type of parent class.
```python
class Vehicle:
    pass

class Car(Vehicle):
    pass

my_car = Car()
print(isinstance(my_car, Car))      # True
print(isinstance(my_car, Vehicle))  # True (Car IS-A Vehicle)
```

The `isinstance()` function confirms the relationship between child and parent classes.
