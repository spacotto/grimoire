# About Variables
A variable is a **named container that stores data in your program**. 

>[!TIP]
>Think of it as a labeled box where you can put information and retrieve it later.

## Creating Variables
Python uses simple assignment to create variables. No declaration is needed.
```
first_name = "Jane"
last_name = "Doe"
age = 25
height = 1.60
is_student = True
```

>[!IMPORTANT]
>Variables are created the moment you assign a value to them.

## Naming Rules
**Must:**
- Start with a letter (`a-z`, `A-Z`) or underscore (`_`)
- Contain only letters, numbers, and underscores
- Are case-sensitive (`age` and `Age` are different)

**Must NOT:**
- Start with a number
- Contain spaces or special characters (`!`, `@`, `#`, etc.)
- Use Python keywords (like `if`, `for`, `class`)

### Examples
**Valid:**
```
user_name = "Bob"
total_count = 100
_private_var = "hidden"
firstName = "John"
```

**NOT valid:**
```
2nd_place = "Invalid"  # starts with number
my-var = 5             # contains hyphen
class = "Math"         # uses keyword
```

## Variable Types
Python automatically determines the type based on the value assigned.
```
x = 10             # int (integer)
y = 3.14           # float (decimal)
name = "Python"    # str (string)
is_valid = True    # bool (boolean)
items = [1,2,3]    # list
data = None        # NoneType
```

>[!TIP]
>Check a variable's type:
>```
>type(x)  # returns <class 'int'>
>```

## Reassigning Variables
Variables can be reassigned to new values, even of different types.
```
x = 10        # x is an integer
x = "hello"   # now x is a string
```

## Multiple Assignment
Assign multiple variables at once:
```
a, b, c = 1, 2, 3
x = y = z = 0
```

## Variable Scope
### Local variables
Exists only inside a function
```
def my_function():
    local_var = 5  # only accessible here
```

### Global variables
Exists throughout the program
```
global_var = 10

def my_function():
    print(global_var)  # can read global
```

To modify a global variable inside a function:
```
count = 0

def increment():
    global count
    count += 1
```

## Constants
Python doesn't have built-in constants, but by convention, use uppercase names for values that shouldn't change.
```
PI = 3.14159
MAX_SIZE = 100
```

>[!WARNING]
>Unlike C, Python does NOT have macros (`#define`). Constant global variables are the closest elements to C macros.

## Best Practices
- Use descriptive names: `user_age` instead of `x`
- Follow naming conventions: lowercase with underscores for variables (`my_variable`)
- Don't use single letters except for counters (`i`, `j`, `k`) or mathematical formulas
- Keep names concise but clear
