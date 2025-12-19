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
