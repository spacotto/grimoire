# Python Magic Methods Cheat Sheet
**Magic methods** (**dunder methods**) are special methods with double underscores that let you define how objects behave with built-in operations.

## Object Lifecycle
### `__new__(cls, *args, **kwargs)`
Creates a new instance (called before `__init__`).
```python
class Singleton:
    _instance = None
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

### `__init__(self, *args, **kwargs)`
Initializes a new instance.
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

### `__del__(self)`
Called when object is about to be destroyed (destructor).
```python
def __del__(self):
    print("Object deleted")
```

## String Representation
### `__str__(self)`
Informal, readable string (for `str()` and `print()`).
```python
def __str__(self):
    return f"Person: {self.name}"
```

### `__repr__(self)`
Official, unambiguous string (for debugging, `repr()`).
```python
def __repr__(self):
    return f"Person(name={self.name!r}, age={self.age})"
```

### `__format__(self, format_spec)`
Custom string formatting with f-strings.
```python
def __format__(self, spec):
    if spec == 'full':
        return f"{self.name} ({self.age} years old)"
    return str(self)
# Usage: f"{person:full}"
```

## Comparison Operators
### `__eq__(self, other)` - Equal (`==`)
```python
def __eq__(self, other):
    return self.name == other.name
```

### `__ne__(self, other)` - Not equal (`!=`)
```python
def __ne__(self, other):
    return not self.__eq__(other)
```

### `__lt__(self, other)` - Less than (`<`)
```python
def __lt__(self, other):
    return self.age < other.age
```

### `__le__(self, other)` - Less than or equal (`<=`)
### `__gt__(self, other)` - Greater than (`>`)
### `__ge__(self, other)` - Greater than or equal (`>=`)

## Arithmetic Operators
### `__add__(self, other)` - Addition (`+`)
```python
def __add__(self, other):
    return Vector(self.x + other.x, self.y + other.y)
```

### `__sub__(self, other)` - Subtraction (`-`)
### `__mul__(self, other)` - Multiplication (`*`)
### `__truediv__(self, other)` - Division (`/`)
### `__floordiv__(self, other)` - Floor division (`//`)
### `__mod__(self, other)` - Modulo (`%`)
### `__pow__(self, other)` - Power (`**`)

## Reverse Arithmetic (Right-hand operations)

### `__radd__(self, other)` - Reverse add
Called when left operand doesn't support the operation.
```python
# If other + self fails, tries self.__radd__(other)
def __radd__(self, other):
    return self.__add__(other)
```

### `__rsub__`, `__rmul__`, `__rtruediv__`, etc.
Same pattern for other operators.

## In-place Operators
### `__iadd__(self, other)` - In-place add (`+=`)
```python
def __iadd__(self, other):
    self.value += other.value
    return self  # Must return self
```

### `__isub__`, `__imul__`, `__itruediv__`, etc.

## Unary Operators
### `__neg__(self)` - Negation (`-x`)
```python
def __neg__(self):
    return Vector(-self.x, -self.y)
```

### `__pos__(self)` - Positive (`+x`)
### `__abs__(self)` - Absolute value (`abs(x)`)
### `__invert__(self)` - Bitwise NOT (`~x`)

## Container Emulation
### `__len__(self)` - Length (`len()`)
```python
def __len__(self):
    return len(self.items)
```

### `__getitem__(self, key)` - Get item (`obj[key]`)
```python
def __getitem__(self, index):
    return self.items[index]
```

### `__setitem__(self, key, value)` - Set item (`obj[key] = value`)
```python
def __setitem__(self, index, value):
    self.items[index] = value
```

### `__delitem__(self, key)` - Delete item (`del obj[key]`)
```python
def __delitem__(self, index):
    del self.items[index]
```

### `__contains__(self, item)` - Membership (`in`)
```python
def __contains__(self, item):
    return item in self.items
```

## Iteration
### `__iter__(self)` - Make iterable (for loops)
```python
def __iter__(self):
    return iter(self.items)
```

### `__next__(self)` - Iterator protocol
```python
def __next__(self):
    if self.index >= len(self.items):
        raise StopIteration
    value = self.items[self.index]
    self.index += 1
    return value
```

### `__reversed__(self)` - Reverse iteration (`reversed()`)
```python
def __reversed__(self):
    return reversed(self.items)
```

## Callable Objects
### `__call__(self, *args, **kwargs)` - Make object callable
```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor
    
    def __call__(self, x):
        return x * self.factor

times_two = Multiplier(2)
print(times_two(5))  # 10
```

## Context Managers
### `__enter__(self)` - Enter context (`with` statement)
```python
def __enter__(self):
    self.file = open(self.filename, 'r')
    return self.file
```

### `__exit__(self, exc_type, exc_val, exc_tb)` - Exit context
```python
def __exit__(self, exc_type, exc_val, exc_tb):
    self.file.close()
    return False  # Don't suppress exceptions
```

## Attribute Access
### `__getattr__(self, name)` - Called when attribute not found
```python
def __getattr__(self, name):
    return f"Attribute {name} doesn't exist"
```

### `__setattr__(self, name, value)` - Set attribute
```python
def __setattr__(self, name, value):
    super().__setattr__(name, value)
```

### `__delattr__(self, name)` - Delete attribute
```python
def __delattr__(self, name):
    super().__delattr__(name)
```

### `__getattribute__(self, name)` - Called for ALL attribute access
```python
def __getattribute__(self, name):
    # Be careful - can cause infinite recursion
    return super().__getattribute__(name)
```

## Type Conversion
### `__int__(self)` - Convert to int (`int()`)
```python
def __int__(self):
    return int(self.value)
```

### `__float__(self)` - Convert to float (`float()`)
### `__complex__(self)` - Convert to complex
### `__bool__(self)` - Convert to bool (`bool()`, truthiness)
```python
def __bool__(self):
    return len(self.items) > 0
```

### `__bytes__(self)` - Convert to bytes (`bytes()`)
### `__hash__(self)` - Hash value (`hash()`, for sets/dicts)
```python
def __hash__(self):
    return hash((self.x, self.y))
```

## Other Useful Methods
### `__dir__(self)` - List attributes (`dir()`)
```python
def __dir__(self):
    return ['x', 'y', 'custom_method']
```

### `__sizeof__(self)` - Memory size (`sys.getsizeof()`)
### `__class__` - Reference to the class
### `__dict__` - Object's attribute dictionary

## Common Patterns
### Making an immutable object hashable
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    def __hash__(self):
        return hash((self.x, self.y))
```

### Making an object sortable
```python
class Student:
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade
    
    def __lt__(self, other):
        return self.grade < other.grade
```

### Making an object look like a list
```python
class CustomList:
    def __init__(self):
        self.items = []
    
    def __len__(self):
        return len(self.items)
    
    def __getitem__(self, index):
        return self.items[index]
    
    def __setitem__(self, index, value):
        self.items[index] = value
```
