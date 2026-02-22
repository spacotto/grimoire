PART I: PYTHON FUNDAMENTALS
================================================================================
This part introduces you to Python programming from scratch. You'll learn what Python is, how to write and run your first programs, and understand the basic building blocks of code. By the end, you'll be comfortable reading input, displaying output, and understanding how Python programs are structured. This foundation is essential for everything that follows.

[Chapter 1: Introduction to Python](https://github.com/spacotto/grimoire/blob/main/python/01/chapter01.md)
--------------------------------------------------------------------------------
1. What is Python
2. Python Philosophy and Design Principles
3. Setting Up Your Python Environment (`Python 3.1.0+`)
4. Code Quality Standards and `PEP 8`
5. Using the `flake8` Linter

[Chapter 2: Program Structure and Execution](https://github.com/spacotto/grimoire/blob/main/python/01/chapter02.md)
--------------------------------------------------------------------------------
1. How Python Programs Execute
2. The if `__name__ == "__main__"`: Pattern
3. Understanding `__name__` and Module Execution
4. Shebang Lines and Script Permissions
5. When to Use Main Blocks vs. Functions

[Chapter 3: Basic Input and Output](https://github.com/spacotto/grimoire/blob/main/python/01/chapter03.md)
--------------------------------------------------------------------------------
1. The `print()` Function
2. Basic Output Formatting
3. The `input()` Function
4. Reading User Input
5. Input Prompts and User Interaction


PART II: VARIABLES AND DATA TYPES
================================================================================
Variables are containers that store data in your programs. This part teaches you how to create variables, understand different types of data (numbers, text, etc.), and work with strings. You'll learn how Python handles different kinds of information and how to manipulate text effectively. These concepts are fundamental to every program you'll ever write.

[Chapter 4: Variables and Assignment](https://github.com/spacotto/grimoire/blob/main/python/02/chapter04.md)
--------------------------------------------------------------------------------
1. Understanding Variables
2. Variable Assignment and the `=` Operator
3. Naming Conventions (`snake_case`)
4. Valid Variable Names
5. Reassigning Variables

Chapter 5: Primitive Data Types
--------------------------------------------------------------------------------
1. Integers (`int`)
2. Strings (`str`)
3. Understanding Type in Python
4. Type Conversion
5. Converting Strings to Integers: `int()`
6. Converting Numbers to Strings: `str()`

Chapter 6: String Operations
--------------------------------------------------------------------------------
1. String Concatenation
2. String Methods
3. Case Manipulation (`capitalize()`, `upper()`, `lower()`)
4. String Formatting
5. F-strings (Formatted String Literals)
6. Combining Different Data Types in Output


PART III: OPERATORS AND EXPRESSIONS
================================================================================
Operators are the symbols that perform operations on data (`+`, `-`, `*`, `==`, etc.). This part shows you how to do math, compare values, and build complex expressions. You'll learn the order in which Python evaluates operations and how to use comparison operators to make decisions. Mastering operators is essential for writing logic in your programs.

Chapter 7: Arithmetic Operations
--------------------------------------------------------------------------------
1. Basic Arithmetic Operators (`+`, `-`, `*`, `/`)
2. Addition and Subtraction
3. Multiplication and Division
4. Performing Calculations with Variables
5. Storing Calculation Results

Chapter 8: Comparison Operators
--------------------------------------------------------------------------------
1. Greater Than (`>`)
2. Less Than (`<`)
3. Equal To (`==`)
4. Not Equal To (`!=`)
5. Greater Than or Equal (`>=`)
6. Less Than or Equal (`<=`)
7. Boolean Results


PART IV: CONTROL FLOW
================================================================================
Control flow determines the order in which your program executes code. This part teaches you how to make decisions with if/elif/else statements, repeat actions with loops (while and for), and use recursion where functions call themselves. These tools let you write programs that adapt to different situations and process data efficiently. 

Chapter 9: Conditional Statements
--------------------------------------------------------------------------------
1. Making Decisions in Code
2. The `if` Statement
3. The `else` Clause
4. The `elif` Statement
5. Nested Conditionals
6. Boolean Logic in Conditionals

Chapter 10: Loops and Iteration
--------------------------------------------------------------------------------
1. Understanding Repetition
2. The `for` Loop
3. The `range()` Function
4. Loop Variables and Counters
5. Iterating Through Collections

Chapter 11: Recursion
--------------------------------------------------------------------------------
1. What is Recursion?
2. Base Cases and Recursive Cases
3. Helper Functions for Recursion
4. Default Parameter Values in Recursion
5. Iteration vs. Recursion
6. When to Use Recursion


PART V: FUNCTIONS
================================================================================
Functions are reusable blocks of code that perform specific tasks. This part shows you how to create functions, pass data to them through parameters, return results, and add type hints for clarity. You'll learn to write modular, organised code that's easy to test and maintain. Functions are the building blocks of larger programs.

Chapter 12: Function Basics
--------------------------------------------------------------------------------
1. What are Functions?
2. Defining Functions with def
3. Function Naming Conventions (snake_case)
4. Calling Functions
5. Function Parameters
6. Return Values

Chapter 13: Type Hints and Annotations
--------------------------------------------------------------------------------
1. Introduction to Type Hints
2. Annotating Function Parameters
3. Return Type Annotations
4. The None Type
5. Benefits of Type Hints
6. Type Hints for Collections

Chapter 14: Documentation
--------------------------------------------------------------------------------
1. Writing Docstrings
2. Docstring Conventions
3. Comments vs. Docstrings
4. Documentation Best Practices


PART VI: OBJECT-ORIENTED PROGRAMMING
================================================================================
Object-oriented programming (OOP) organises code around objects that contain both data (attributes) and behaviour (methods). This part introduces classes as blueprints for objects, teaches encapsulation to protect data, and shows how inheritance lets you build on existing code. OOP is essential for building complex, maintainable applications. Then, you'll master polymorphism (objects responding differently to the same method), method overriding, abstract base classes (ABCs) that enforce contracts, and protocols for duck typing. These techniques are used in professional software development to create extensible, maintainable systems. At last, you will learn that abstract programming focuses on defining "what" objects should do, not "how" they do it. This part teaches interface design principles, multiple interface implementations, and powerful design patterns like Abstract Factory and Strategy. You'll learn to build flexible systems that can evolve and adapt without breaking existing code. These are enterprise-level skills used in large-scale software development.

Chapter 15: Introduction to OOP
--------------------------------------------------------------------------------
1. What is Object-Oriented Programming?
2. Objects in the Real World
3. OOP vs. Procedural Programming
4. Benefits of OOP
5. When to Use OOP

Chapter 16: Classes and Objects
--------------------------------------------------------------------------------
1. Classes as Blueprints
2. Objects as Instances
3. The class Keyword
4. Class Naming Conventions (`PascalCase`)
5. Creating Your First Class
6. Instantiating Objects
7. Multiple Instances

Chapter 17: Attributes and Instance Variables
--------------------------------------------------------------------------------
1. Understanding Attributes
2. Instance Variables
3. The self Parameter
4. Accessing Attributes (`self.attribute`)
5. Modifying Attributes
6. Class Variables vs. Instance Variables

Chapter 18: Methods
--------------------------------------------------------------------------------
1. What are Methods?
2. Defining Instance Methods
3. The self Parameter in Methods
4. Calling Methods on Objects
5. Methods vs. Functions
6. Methods that Modify State
7. Methods that Return Information

Chapter 19: Constructors and Initialisation
--------------------------------------------------------------------------------
1. The `__init__()` Method
2. Constructor Parameters
3. Initialising Instance Variables
4. Default Parameter Values in Constructors
5. Object Creation Process
6. Factory Pattern Basics

Chapter 20: Encapsulation
--------------------------------------------------------------------------------
1. Understanding Encapsulation
2. Data Protection and Information Hiding
3. Public vs. Private Attributes
4. Name Mangling with Double Underscores
5. Getter Methods
6. Setter Methods
7. Data Validation in Setters
8. Why Encapsulation Matters
9. Protecting Data Integrity

Chapter 21: Inheritance
--------------------------------------------------------------------------------
1. What is Inheritance?
2. Parent Classes (Base Classes)
3. Child Classes (Derived Classes)
4. The `super()` Function
5. Calling Parent Constructors
6. Overriding Methods
7. Extending Parent Functionality
8. Adding New Methods in Child Classes
9. Adding New Attributes in Child Classes
10. Inheritance Hierarchies
11. Multi-Level Inheritance
12. Code Reusability Through Inheritance
13. `IS-A` Relationships

Chapter 22: Advanced Method Types
--------------------------------------------------------------------------------
1. Instance Methods (Review)
2. Class Methods
   1. The `@classmethod` Decorator
   2. The `cls` Parameter
   3. When to Use Class Methods
3. Static Methods
   1. The `@staticmethod` Decorator
   2 Methods Without `self` or `cls`
   3. When to Use Static Methods
4. Choosing the Right Method Type

Chapter 23: Nested Classes and Composition
--------------------------------------------------------------------------------
1. What are Nested Classes?
2. Defining Classes Within Classes
3. When to Use Nested Classes
4. Accessing Nested Classes
5. Helper Classes and Organisation
6. Namespace Management
7. Composition vs. Inheritance
8. `HAS-A` Relationships

Chapter 24: Polymorphism Fundamentals
--------------------------------------------------------------------------------
1. What is Polymorphism?
2. Types of Polymorphism
3. Subtype Polymorphism (Inheritance-based)
4. Duck Typing in Python
5. Polymorphic Behaviour
6. Interface Consistency
7. Same Interface, Different Behaviour
8. Benefits of Polymorphic Design

Chapter 25: Method Overriding
--------------------------------------------------------------------------------
1. What is Method Overriding?
2. Overriding vs. Overloading
3. Method Signatures and Compatibility
4. Overriding Parent Methods
5. Calling Parent Methods with super()
6. Behavioural Specialisation
7. When to Override Methods
8. Method Resolution Order (MRO)

Chapter 26: Abstract Base Classes (ABC)
--------------------------------------------------------------------------------
1. What are Abstract Base Classes (ABC)?
2. The `abc` Module
3. The ABC Base Class
4. The `@abstractmethod` Decorator
5. Defining Abstract Methods
6. Implementing Abstract Methods in Subclasses
7. Cannot Instantiate Abstract Classes
8. Abstract Properties
9. Enforcing Interface Contracts
10. When to Use Abstract Base Classes
11. Abstract Methods vs. Concrete Methods
12. Partial Implementation in Abstract Classes

Chapter 27: Protocols and Duck Typing
--------------------------------------------------------------------------------
1. What are Protocols?
2. Structural Subtyping
3. The `typing.Protocol` Class
4. Defining Protocol Interfaces
5. Duck Typing ("If it walks like a duck...")
6. Protocol vs. ABC
7. Runtime Checkable Protocols
8. When to Use Protocols

Chapter 28: Advanced Type Hints
--------------------------------------------------------------------------------
1. The typing Module
2. Generic Types (`List`, `Dict`, `Set`, `Tuple`)
3. Union Types
4. Optional Types
5. Any Type
6. Type Aliases
7. Generic Classes and Functions
8. Type Hints for Polymorphic Code

Chapter 29: Polymorphic Design Patterns
--------------------------------------------------------------------------------
1. Strategy Pattern
2. Template Method Pattern
3. Factory Pattern with Polymorphism
4. Adapter Pattern
5. Pipeline Pattern
6. Composition with Polymorphism
7. Dependency Injection
8. Interface Segregation

Chapter 30: Building Polymorphic Systems
--------------------------------------------------------------------------------
1. Designing for Extensibility
2. Open/Closed Principle
3. Liskov Substitution Principle
4. Interface-Based Programming
5. Processing Mixed Types Polymorphically
6. Batch Processing with Polymorphism
7. Error Handling in Polymorphic Systems
8. Performance Considerations

Chapter 31: Advanced Inheritance Patterns
--------------------------------------------------------------------------------
1. Multiple Inheritance
2. Method Resolution Order (MRO) in Detail
3. Mixin Classes
4. Diamond Problem
5. Cooperative Multiple Inheritance
6. Abstract vs. Concrete Methods
7. Inheritance Hierarchies Design
8. When to Use Composition Over Inheritance

Chapter 32: Interface Design Principles
--------------------------------------------------------------------------------
1. What are Interfaces?
2. Interfaces vs. Abstract Classes
3. Interface Segregation Principle
4. Designing Minimal Interfaces
5. Interface Composition
6. Cohesive Interface Design
7. Contract-Based Programming
8. Interface Documentation

Chapter 33: Multiple Interface Implementation
--------------------------------------------------------------------------------
1. Implementing Multiple Interfaces
2. Combining Behaviours Through Interfaces
3. Interface Composition Patterns
4. Managing Method Name Conflicts
5. Interface Hierarchies
6. When to Use Multiple Interfaces
7. Benefits of Multiple Interface Design
8. Common Pitfalls and Solutions

Chapter 34: Abstract Factory Pattern
--------------------------------------------------------------------------------
1. What is the Abstract Factory Pattern?
2. Factory Method vs. Abstract Factory
3. Defining Abstract Factory Interfaces
4. Concrete Factory Implementations
5. Product Families
6. Creating Related Objects
7. Factory Registration and Discovery
8. When to Use Abstract Factory
9. Benefits and Trade-offs

Chapter 35: Strategy Pattern
--------------------------------------------------------------------------------
1. What is the Strategy Pattern?
2. Defining Strategy Interfaces
3. Concrete Strategy Implementations
4. Context and Strategy Interaction
5. Runtime Strategy Selection
6. Strategy Composition
7. When to Use Strategy Pattern
8. Benefits and Trade-offs

Chapter 36: Combining Design Patterns
--------------------------------------------------------------------------------
1. Factory + Strategy Combination
2. Abstract Factory + Template Method
3. Strategy + Decorator Patterns
4. Composing Multiple Patterns
5. Pattern Interaction and Communication
6. Avoiding Over-Engineering
7. Practical Pattern Combinations
8. Real-World Pattern Usage

Chapter 37: Building Flexible Systems
--------------------------------------------------------------------------------
1. Plugin Architectures
2. Extensible System Design
3. Hot-Swappable Components
4. Configuration-Driven Behaviour
5. Dynamic Feature Registration
6. Versioning and Compatibility
7. Migration Strategies
8. Backward Compatibility

Chapter 38: Advanced Abstraction Techniques
--------------------------------------------------------------------------------
1. Layered Abstractions
2. Abstraction Levels
3. Leaky Abstractions
4. Abstraction Trade-offs
5. When to Abstract
6. Over-Abstraction Pitfalls
7. Balancing Flexibility and Simplicity
8. Refactoring Toward Abstractions


PART VII: DATA STRUCTURES AND COLLECTIONS
================================================================================
Data structures are containers that hold and organise data in different ways. This part covers Python's built-in collections: lists (ordered, mutable), tuples (immutable), sets (unique items), and dictionaries (key-value pairs). You'll also learn generators for memory-efficient iteration and comprehensions for concise data transformation. Choosing the right data structure is crucial for writing efficient code.

Chapter 39: Command-Line Arguments
--------------------------------------------------------------------------------
1. The sys Module
2. Understanding `sys.argv`
3. Accessing Command-Line Arguments
4. Program Name vs. Arguments
5. Processing Multiple Arguments
6. Command-Line Data Processing

Chapter 40: Lists
--------------------------------------------------------------------------------
1. What are Lists?
2. Creating Lists
3. List Indexing and Slicing
4. Adding Elements (`append`, `insert`, `extend`)
5. Removing Elements (`remove`, `pop`, `clear`)
6. List Operations (`concatenation`, `repetition`)
7. Common List Methods
8. List Iteration
9. Built-in Functions for Lists (`len`, `sum`, `max`, `min`)
10. Sorting Lists (`sort`, `sorted`)
11. List Comprehensions (Preview)
12. When to Use Lists

Chapter 41: Tuples
--------------------------------------------------------------------------------
1. What are Tuples?
2. Creating Tuples
3. Tuple Immutability
4. Accessing Tuple Elements
5. Tuple Unpacking
6. Multiple Assignment with Tuples
7. Tuples as Return Values
8. Common Tuple Operations
9. Named Tuples
10. When to Use Tuples
11. Tuples vs. Lists

Chapter 42: Sets
--------------------------------------------------------------------------------
1. What are Sets?
2. Creating Sets
3. Set Uniqueness Property
4. Adding and Removing Elements
5. Set Operations
   1. Union (`|`)
   2. Intersection (`&`)
   3. Difference (`-`)
   4. Symmetric Difference (`^`)
6. Set Methods (union, intersection, difference)
7. Subset and Superset Operations
8. Set Comprehensions (Preview)
9. When to Use Sets
10. Practical Set Applications

Chapter 43: Dictionaries
--------------------------------------------------------------------------------
1. What are Dictionaries?
2. Creating Dictionaries
3. Key-Value Pairs
4. Accessing Values
5. Adding and Updating Items
6. Removing Items (`del`, `pop`, `popitem`, `clear`)
7. Dictionary Methods
   1. `keys()`
   2. `values()`
   3. `items()`
   4. `get()`
   5. `update()`
8. Checking for Key Existence
9. Iterating Over Dictionaries
10. Nested Dictionaries
11. Dictionary Comprehensions (Preview)
12. When to Use Dictionaries

Chapter 44: Generators and Iteration
--------------------------------------------------------------------------------
1. What are Generators?
2. The `yield` Keyword
3. Generator Functions
4. Generator Expressions
5. `next()` and `iter()`
6. Lazy Evaluation
7. Memory Efficiency with Generators
8. Generator vs. List Performance
9. Infinite Generators
10. Generator Patterns
11. When to Use Generators
12. The `typing.Generator` Type Hint

Chapter 45: Comprehensions
--------------------------------------------------------------------------------
1. What are Comprehensions?
2. List Comprehensions
   1. Basic Syntax
   2. Filtering with Conditions
   3. Transforming Data
   4. Nested List Comprehensions
3. Dictionary Comprehensions
   1. Creating Dictionaries from Sequences
   2. Filtering Dictionaries
   3. Transforming Keys and Values
4. Set Comprehensions
   1. Creating Sets from Sequences
   2. Deduplication with Comprehensions
5. Generator Expressions (vs. List Comprehensions)
6. When to Use Comprehensions
7. Readability vs. Complexity
8. Performance Considerations

Chapter 46: Working with Collections
--------------------------------------------------------------------------------
1. Choosing the Right Data Structure
2. Collection Performance Characteristics
3. Common Collection Patterns
4. Combining Different Collection Types
5. Data Transformation Pipelines
6. Nested Data Structures
7. Collection Best Practices


PART VIII: MODULES AND PACKAGES
================================================================================
As programs grow, organising code becomes essential. This part teaches you how to split code into modules (files) and packages (directories), import code from other files, and manage dependencies between modules. You'll understand absolute vs. relative imports, avoid circular dependencies, and use __init__.py to control package interfaces. This is fundamental to writing maintainable, professional Python projects.

Chapter 47: Introduction to Modules
--------------------------------------------------------------------------------
1. What are Modules?
2. Why Use Modules?
3. Module Files (`.py`)
4. The Module Search Path
5. How Python Finds Modules
6 Code Organisation Benefits

Chapter 48: Basic Import Statements
--------------------------------------------------------------------------------
1. The import Statement
2. Importing Entire Modules
3. Using Module Functions (`module.function()`)
4. Importing Multiple Modules
5. Import Statement Placement
6. Module Namespaces

Chapter 49: from...import Statements
--------------------------------------------------------------------------------
1. Importing Specific Functions
2. `from` module `import` function
3. Importing Multiple Items
4. `from` module `import *`
5. Why to Avoid `import *`
6. Namespace Considerations

Chapter 50: Import Aliases
--------------------------------------------------------------------------------
1. The `as` Keyword
2. `import module as alias`
3. `from module import function as alias`
4. When to Use Aliases
5. Common Aliasing Conventions
6. Improving Code Readability

Chapter 51: Packages
--------------------------------------------------------------------------------
1. What are Packages?
2. Package Directories
3. The `__init__.py` File
4. Package Initialisation
5. Creating Your First Package
6. Nested Packages (Subpackages)
7. Package Structure Best Practices

Chapter 52: The `__init__.py` Sacred Scroll
--------------------------------------------------------------------------------
1. Purpose of `__init__.py`
2. Empty vs. Populated `__init__.py`
3. Controlling Package Interface
4. Exposing Functions at Package Level
5. Package Metadata (`__version__`, `__author__`)
6. Selective Import Exposure
7. Package-Level vs. Module-Level Access
8. Information Hiding with `__init__.py`

Chapter 53: Absolute Imports
--------------------------------------------------------------------------------
1. What are Absolute Imports?
2. Full Import Paths
3. `from package.module import function`
4. Clarity and Explicitness
5. When to Use Absolute Imports
6. Absolute Import Best Practices

Chapter 54: Relative Imports
--------------------------------------------------------------------------------
1. What are Relative Imports?
2. Dot Notation (`.` and `..`)
3. `from . import module`
4. `from .. import module`
5. Sibling Module Imports
6. Parent Package Imports
7. When to Use Relative Imports
8. Relative Import Limitations

Chapter 55: Absolute vs. Relative Imports
--------------------------------------------------------------------------------
1. The Great Pathway Debate
2. Advantages of Absolute Imports
3. Advantages of Relative Imports
4. Clarity vs. Conciseness
5. Refactoring Considerations
6. Project Size Considerations
7. Team Preferences
8. PEP 8 Recommendations

Chapter 56: Circular Dependencies
--------------------------------------------------------------------------------
1. What are Circular Dependencies?
2. The Circular Import Problem
3. Why Circular Imports Fail
4. Detecting Circular Dependencies
5. Circular Dependency Patterns
6. The Danger of Circular Imports

Chapter 57: Breaking Circular Dependencies
--------------------------------------------------------------------------------
1. Late Imports (Import Inside Functions)
2. Dependency Injection
3. Shared/Common Modules
4. Restructuring Code
5. Interface Modules
6. Choosing the Right Solution
7. Prevention Strategies
8. Design Patterns to Avoid Circularity

Chapter 58: Module and Package Best Practices
--------------------------------------------------------------------------------
1. Organising Code into Modules
2. When to Create a Package
3. Flat vs. Nested Package Structures
4. Module Naming Conventions
5. Package Naming Conventions
6. Import Statement Organisation
7. Avoiding Common Import Pitfalls
8. Documentation for Modules and Packages


PART IX: FILE I/O AND STREAMS
================================================================================
Programs often need to read data from files and write results back. This part teaches file operations: reading, writing, and using context managers (with statement) for safe file handling. You'll learn about stdin, stdout, and stderr streams, and best practices for resource management. File I/O is essential for data processing, logging, and persistent storage.

Chapter 59: Introduction to File Operations
--------------------------------------------------------------------------------
1. What is File I/O?
2. Why File Operations Matter
3. File Paths and Locations
4. Text Files vs. Binary Files
5. File Operations Overview
6. Common File Operation Pitfalls

Chapter 60: Reading Files
--------------------------------------------------------------------------------
1. The open() Function
2. File Modes (`'r'`, `'w'`, `'a'`, `'r+'`)
3. Reading Entire Files (`read()`)
4. Reading Line by Line (`readline()`)
5. Reading All Lines (`readlines()`)
6. File Objects and Iteration
7. Closing Files with `close()`
8. File Encoding

Chapter 61.: Writing Files
--------------------------------------------------------------------------------
1. Opening Files for Writing
2. Write Mode vs. Append Mode
3. The `write()` Method
4. The `writelines()` Method
5. Overwriting vs. Appending
6. Flushing Buffers
7. File Permissions

Chapter 62: Context Managers and the with Statement
--------------------------------------------------------------------------------
1. What are Context Managers?
2. The `with` Statement
3. Automatic Resource Management
4. RAII Principle (Resource Acquisition Is Initialisation)
5. Why `with` is Essential
6. Context Managers `with` Files
7. Multiple Files in `with` Statements
8. Creating Custom Context Managers
9. Exception Safety `with with`

Chapter 63: Standard Streams
--------------------------------------------------------------------------------
1. Understanding Standard I/O
2. Standard Input (`stdin`)
3. Standard Output (`stdout`)
4. Standard Error (`stderr`)
5. Reading `from sys.stdin`
6. Writing `to sys.stdout`
7. Writing `to sys.stderr`
8. Stream Redirection
9. When to Use Each Stream
10. Separating Normal Output from Errors

Chapter 64: File Operations Best Practices
--------------------------------------------------------------------------------
1. Always Use Context Managers
2. Handle File Not Found Errors
3. Handle Permission Errors
4. Verify File Existence
5. Resource Cleanup Patterns
6. File Operation Error Handling
7. Performance Considerations
8. Security Considerations


PART X: EXCEPTION HANDLING AND ERROR MANAGEMENT
================================================================================
Errors are inevitable in programming. This part teaches you how to anticipate, catch, and handle errors gracefully using try/except blocks. You'll learn about Python's built-in exceptions, how to create custom exceptions, and best practices for error handling. Good exception handling makes your programs robust and user-friendly instead of crashing unexpectedly.

[Chapter 65: Introduction to Exception Handling](https://github.com/spacotto/grimoire/blob/main/python/10/chapter65.md)
--------------------------------------------------------------------------------
1. What are Exceptions?
2. Why Exception Handling Matters
3. Errors vs. Exceptions
4. The Cost of Unhandled Exceptions
5. Defensive Programming Principles
6. Building Robust Applications

[Chapter 66: Basic Exception Handling](https://github.com/spacotto/grimoire/blob/main/python/10/chapter66.md)
--------------------------------------------------------------------------------
1. The `try` Block
2. The `except` Block
3. Catching Specific Exceptions
4. Basic Error Recovery
5. Continuing Execution After Errors
6. Exception Handling Flow

[Chapter 67: Built-in Exception Types](https://github.com/spacotto/grimoire/blob/main/python/10/chapter67.md)
--------------------------------------------------------------------------------
1. `ValueError` - Invalid Data
2. `TypeError` - Wrong Type of Data
3. `ZeroDivisionError` - Division by Zero
4. `FileNotFoundError` - Missing Files
5. `PermissionError` - Access Denied
6. `KeyError` - Missing Dictionary Keys
7. `IndexError` - List Index Out of Range
8. `AttributeError` - Missing Attributes
9. `ImportError` and `ModuleNotFoundError`
10. The Exception Hierarchy
11. Choosing the Right Exception Type

[Chapter 68: Multiple Exception Handling](https://github.com/spacotto/grimoire/blob/main/python/10/chapter68.md)
--------------------------------------------------------------------------------
1. Catching Multiple Exception Types
2. Multiple except Blocks
3. Catching Multiple Exceptions in One Block
4. Exception Handler Order
5. Specific vs. General Exception Handlers
6. Best Practices for Multiple Handlers

[Chapter 69: Custom Exceptions](https://github.com/spacotto/grimoire/blob/main/python/10/chapter69.md)
--------------------------------------------------------------------------------
1. When to Create Custom Exceptions
2. Defining Custom Exception Classes
3. Inheriting from Exception
4. Creating Exception Hierarchies
5. Custom Error Messages
6. Adding Custom Attributes to Exceptions
7. Organising Domain-Specific Exceptions
8. Benefits of Custom Exceptions

[Chapter 70: The finally Block](https://github.com/spacotto/grimoire/blob/main/python/10/chapter70.md)
--------------------------------------------------------------------------------
1. What is the `finally` Block?
2. Guaranteed Cleanup with `finally`
3. Resource Management
4. `finally` vs. `except`
5. When finally Always Executes
6. Cleanup Patterns
7. File and Connection Cleanup
8. Combining with Context Managers

[Chapter 71: Raising Exceptions](https://github.com/spacotto/grimoire/blob/main/python/10/chapter71.md)
--------------------------------------------------------------------------------
1. The `raise` Keyword
2. When to Raise Exceptions
3. Raising Built-in Exceptions
4. Raising Custom Exceptions
5. Creating Helpful Error Messages
6. Re-raising Exceptions
7. Exception Chaining
8. Input Validation with Exceptions

[Chapter 72: Exception Handling Best Practices](https://github.com/spacotto/grimoire/blob/main/python/10/chapter72.md)
--------------------------------------------------------------------------------
1. Don't Catch Everything
2. Be Specific with Exception Types
3. Fail Fast vs. Defensive Programming
4. Logging Exceptions
5. User-Friendly Error Messages
6. Error Recovery Strategies
7. When Not to Use Exceptions
8. Performance Considerations

[Chapter 73: Data Validation and Integrity](https://github.com/spacotto/grimoire/blob/main/python/10/chapter73.md)
--------------------------------------------------------------------------------
1. Input Validation Techniques
2. Data Sanitisation
3. Boundary Checking
4. Type Validation
5. Range Validation
6. Format Validation
7. Maintaining Data Integrity

[Chapter 74: Combining Exception Handling with File I/O](https://github.com/spacotto/grimoire/blob/main/python/10/chapter74.md)
--------------------------------------------------------------------------------
1. File Operations and Error Handling
2. Handling FileNotFoundError
3. Handling PermissionError
4. Handling IOError and OSError
5. Safe File Operations Pattern
6. Using with and try Together
7. Crisis Response in File Systems


PART XI: DESIGN PRINCIPLES AND BEST PRACTICES
================================================================================
Writing code that works is good; writing code that's maintainable is better. This part covers software design principles like SOLID, code organisation strategies, and testing fundamentals. You'll learn how to structure projects, design for change, and build complex systems that remain understandable as they grow. These principles separate professional developers from beginners.

Chapter 75: Code Organisation
--------------------------------------------------------------------------------
1. File Organisation and Naming
2. One Class Per File Guidelines
3. Module Structure
4. Avoiding Global Variables
5. Organising Related Functionality
6. Import Statements and Dependencies
7. Project Structure Best Practices
8. Package Layout Patterns

Chapter 76: Object-Oriented Design Principles
--------------------------------------------------------------------------------
1. Single Responsibility Principle
2. Separation of Concerns
3. DRY Principle (Don't Repeat Yourself)
4. Code Reusability
5. Maintainability
6. Designing for Change
7. Open/Closed Principle
8. Liskov Substitution Principle
9. Interface Segregation Principle
10. Dependency Inversion Principle

Chapter 77: Building Complex Systems
--------------------------------------------------------------------------------
1. Managing Multiple Objects
2. Object Collections
3. Object Relationships
4. System Architecture Planning
5. Scalable Code Design
6. Interacting Components
7. Pipeline Architectures
8. Data Processing Systems
9. Extensible Plugin Systems
10. Modular Architecture Patterns

Chapter 78: Testing and Debugging
--------------------------------------------------------------------------------
1. Testing Functions
2. Testing Classes and Objects
3. Understanding Imports
4. Error Messages and Troubleshooting
5. Test-Driven Development Basics
6. Writing Test Cases
7. Testing Polymorphic Systems
8. Integration Testing
9. Testing Abstract Interfaces
10. Mocking and Test Doubles


PART XII: PYTHON DEVELOPMENT ENVIRONMENT AND TOOLS
================================================================================
Professional Python development requires mastering the ecosystem tools. This part teaches virtual environments (isolating project dependencies), package management with pip and Poetry, environment variables for configuration, and security practices. You'll learn to set up projects professionally, manage dependencies reproducibly, and keep secrets secure. These are essential skills for real-world development and team collaboration.

Chapter 79: Introduction to Python Environments
--------------------------------------------------------------------------------
1. What are Python Environments?
2. Global vs. Local Environments
3. Why Isolated Environments Matter
4. Environment Pollution Problems
5. Dependency Conflicts
6. Reproducible Environments
7. Development vs. Production Environments

Chapter 80: Virtual Environments (venv)
--------------------------------------------------------------------------------
1. What is a Virtual Environment?
2. The venv Module
3. Creating Virtual Environments
4. Activating Virtual Environments
5. Deactivating Virtual Environments
6. Virtual Environment Structure
7. Detecting Virtual Environments Programmatically
8. Virtual Environment Best Practices
9. When to Use Virtual Environments
10. Common Virtual Environment Issues

Chapter 81: Package Management with pip
--------------------------------------------------------------------------------
1. What is `pip`?
2. Installing Packages
3. Uninstalling Packages
4. Listing Installed Packages
5. Upgrading Packages
6. `pip freeze` and Requirements Files
7. `requirements.txt` Format
8. Installing from Requirements Files
9. `pip show` - Package Information
10. pip search and Package Discovery
11. Version Pinning and Constraints
12. pip Best Practices

Chapter 82.: Advanced Package Management with Poetry
--------------------------------------------------------------------------------
1. What is Poetry?
2. Poetry vs. pip
3. Installing Poetry
4. `pyproject.toml` Format
5. `poetry.lock` Files
6. `poetry install`
7. `poetry add` and `poetry remove`
8. `poetry update`
9. Dependency Groups (`dev`, `test`, `docs`)
10. Poetry Virtual Environment Management
11. `poetry run`
12. Publishing Packages with Poetry

Chapter 83: Dependency Management
--------------------------------------------------------------------------------
1. Understanding Dependencies
2. Direct vs. Transitive Dependencies
3. Dependency Resolution
4. Version Constraints and Semantic Versioning
5. Dependency Conflicts
6. Pinning Dependencies
7. `requirements.txt` vs. `pyproject.toml`
8. Lock Files
9. Reproducible Builds
10. Security Considerations

Chapter 84: Environment Variables
--------------------------------------------------------------------------------
1. What are Environment Variables?
2. Reading Environment Variables (`os.environ`)
3. Setting Environment Variables
4. Environment Variables in Different OS
5. Environment Variable Naming Conventions
6. When to Use Environment Variables
7. Security with Environment Variables
8. Environment Variable Precedence

Chapter 85: Configuration Management
--------------------------------------------------------------------------------
1. Configuration vs. Code
2. The `.env` File Format
3. `python-dotenv` Library
4. Loading `.env` Files
5. `.env.example` Templates
6. Multiple Environment Configurations
7. Development vs. Production Config
8. Configuration Hierarchies
9. Configuration Validation
10. Secrets Management

Chapter 86: Security and `.gitignore`
--------------------------------------------------------------------------------
1. Never Commit Secrets
2. The `.gitignore` File
3. `.gitignore` Patterns
4. Protecting `.env` Files
5. Protecting Virtual Environments
6. API Keys and Credentials
7. Security Best Practices
8. Environment-Specific Secrets
9. Secret Rotation
10. Auditing and Compliance

Chapter 87: Python Project Structure
--------------------------------------------------------------------------------
1. Standard Project Layout
2. `src/` Layout vs. Flat Layout
3. Directory Organisation
4. `tests/` Directory
5. `docs/` Directory
6. Configuration Files Location
7. `README` and Documentation
8. LICENSE Files
9. `setup.py` and `pyproject.toml`
10. Project Metadata

Chapter 88: Development Workflow
--------------------------------------------------------------------------------
1. Setting Up New Projects
2. Environment Setup Checklist
3. Dependency Installation Workflow
4. Adding New Dependencies
5. Updating Dependencies
6. Testing in Clean Environments
7. Sharing Projects with Others
8. Onboarding New Developers
9. CI/CD Environment Setup
10. Production Deployment Considerations


PART XIII: DATA VALIDATION AND SERIALIZATION
================================================================================
Validating data at runtime prevents bugs and security issues. This part introduces Pydantic, Python's leading data validation library. You'll learn to create models with automatic validation, define custom validation rules, work with nested data structures, and handle validation errors gracefully. Pydantic is widely used in modern Python applications, especially with FastAPI for building APIs.

Chapter 89: Introduction to Data Validation
--------------------------------------------------------------------------------
1. Why Data Validation Matters
2. Validation vs. Type Checking
3. Runtime Validation
4. Data Integrity and Quality
5. Validation in APIs and Services
6. Common Validation Challenges
7. Manual Validation vs. Libraries

Chapter 90: Introduction to Pydantic
--------------------------------------------------------------------------------
1. What is Pydantic?
2. Why Use Pydantic?
3. Installing Pydantic
4. Pydantic v1. vs. v2.
5. Key Pydantic Features
6. When to Use Pydantic
7. Pydantic in Real-World Applications

Chapter 91: BaseModel Fundamentals
--------------------------------------------------------------------------------
1. The BaseModel Class
2. Creating Your First Model
3. Model Instantiation
4. Automatic Type Conversion
5. Accessing Model Fields
6. Model Serialisation
7. `model_dump()` and `model_dump_json()`
8. Model Comparison
9. Model Copying
10. Model Immutability

Chapter 92: Field Validation
--------------------------------------------------------------------------------
1. The Field Function
2. Field Constraints
3. String Constraints (`min_length`, `max_length`, `pattern`)
4. Numeric Constraints (`ge`, `le`, `gt`, `lt`)
5. Default Values
6. Required vs. Optional Fields
7. Field Descriptions and Metadata
8. Field Aliases
9. Field Examples
10. Computed Fields

Chapter 93: Type Annotations and Validation
--------------------------------------------------------------------------------
1. Basic Type Validation
2. Standard Library Types
3. Optional and Union Types
4. List and Dict Validation
5. Tuple Validation
6. Datetime and Date Validation
7. UUID Validation
8. Email and URL Validation
9. Custom Types
10. Type Coercion

Chapter 94: Enums and Literal Types
--------------------------------------------------------------------------------
1. Python Enum with Pydantic
2. Defining Enums for Models
3. String Enums
4. Integer Enums
5. Literal Types
6. Enum Validation
7. When to Use Enums vs. Literals
8. Enum Serialisation

Chapter 95: Custom Validation
--------------------------------------------------------------------------------
1. Model Validators
2. The `@model_validator` Decorator
3. Before vs. After Mode
4. Field Validators (Deprecated in v2.)
5. Multi-Field Validation
6. Conditional Validation
7. Cross-Field Dependencies
8. Business Logic Validation
9. Validation Error Messages
10. Raising `ValidationError`

Chapter 96: Nested Models
--------------------------------------------------------------------------------
1. Model Composition
2. Nested Model Definition
3. One-to-One Relationships
4. One-to-Many Relationships
5. Lists of Models
6. Nested Model Validation
7. Deep Nesting Considerations
8. Circular References
9. Forward References
10. Model Reusability

Chapter 97: Advanced Pydantic Features
--------------------------------------------------------------------------------
1. Model Configuration
2. Extra Fields Handling
3. Alias Generators
4. JSON Schema Generation
5. Model Validation Context
6. Strict Mode
7. Custom JSON Encoders
8. Serialisation Customisation
9. Root Validators
10. Generic Models

Chapter 98: Error Handling and ValidationError
--------------------------------------------------------------------------------
1. Understanding ValidationError
2. Error Structure
3. Error Messages
4. Error Locations
5. Multiple Validation Errors
6. Handling ValidationError
7. Custom Error Messages
8. Error Formatting
9. Debugging Validation Issues
10. Error Recovery Strategies

Chapter 99: Pydantic Best Practices
--------------------------------------------------------------------------------
1. Model Design Principles
2. Validation Performance
3. Model Organisation
4. Documentation with Models
5. Testing Pydantic Models
6. Avoiding Common Pitfalls
7. Migration from v1. to v2.
8. Integration with FastAPI
9. Database Integration
10. Production Considerations


PART XIV: FUNCTIONAL PROGRAMMING
================================================================================
Functional programming treats computation as evaluating mathematical functions, emphasising immutability and avoiding side effects. This part teaches lambda expressions, higher-order functions (functions that take/return other functions), closures (functions that remember their environment), and decorators (functions that modify other functions). These powerful techniques lead to more concise, testable, and elegant code.

Chapter 100: Introduction to Functional Programming
--------------------------------------------------------------------------------
1. What is Functional Programming?
2. Functional vs. Imperative Programming
3. Functional vs. Object-Oriented Programming
4. Pure Functions
5. Immutability
6. Side Effects
7. Functional Programming in Python
8. Benefits of Functional Programming
9. When to Use Functional Programming

Chapter 101: Lambda Expressions
--------------------------------------------------------------------------------
1. What are Lambda Functions?
2. Lambda Syntax
3. Anonymous Functions
4. Lambda vs. def
5. When to Use Lambda
6. Lambda Limitations
7. Lambda with Built-in Functions
8. Multi-line Lambdas (Avoiding)

Chapter 102: Map, Filter, and Reduce
--------------------------------------------------------------------------------
1. The `map()` Function
2. Mapping Transformations
3. The `filter()` Function
4. Filtering Data
5. The `reduce()` Function
6. `functools.reduce`
7. Combining `map`, `filter`, `reduce`
8. List Comprehensions vs. map/filter
9. Performance Considerations

Chapter 103: First-Class Functions
--------------------------------------------------------------------------------
1. Functions as Objects
2. Assigning Functions to Variables
3. Passing Functions as Arguments
4. Returning Functions from Functions
5. Storing Functions in Data Structures
6. Function Attributes
7. `callable()` Function
8. Function Identity

Chapter 104: Higher-Order Functions
--------------------------------------------------------------------------------
1. What are Higher-Order Functions?
2. Functions Taking Functions
3. Functions Returning Functions
4. Function Composition
5. Function Combinators
6. Practical Higher-Order Functions
7. `sorted()` with key Parameter
8. Custom Higher-Order Functions

Chapter 105: Closures
--------------------------------------------------------------------------------
1. What are Closures?
2. Lexical Scoping
3. Nested Functions
4. Capturing Variables
5. Free Variables
6. Closure Persistence
7. Practical Closure Examples
8. Closures vs. Classes
9. When to Use Closures

Chapter 106: The nonlocal Keyword
--------------------------------------------------------------------------------
1. Understanding nonlocal
2. Modifying Enclosing Scope
3. nonlocal vs. global
4. Stateful Closures
5. Counter Closures
6. Accumulator Patterns
7. Best Practices with nonlocal

Chapter 107: The functools Module
--------------------------------------------------------------------------------
1. Introduction to functools
2. `functools.reduce`
3. `functools.partial`
4. Partial Application
5. `functools.wraps`
6. `functools.lru_cache`
7. Memoisation
8. `functools.singledispatch`
9. `functools.total_ordering`
10. `functools.cache` (Python 3..9+)

Chapter 108: Decorators Fundamentals
--------------------------------------------------------------------------------
1. What are Decorators?
2. Decorator Syntax
3. The `@` Symbol
4. Function Wrappers
5. Preserving Function Metadata
6. functools.wraps Usage
7. Decorator Execution Order
8. Built-in Decorators

Chapter 109: Creating Custom Decorators
--------------------------------------------------------------------------------
1. Basic Decorator Pattern
2. Wrapper Functions
3. `*args` and `**kwargs`
4. Returning Values from Decorated Functions
5. Decorators with Arguments
6. Decorator Factories
7. Parameterised Decorators
8. Class-Based Decorators

Chapter 110: Advanced Decorator Patterns
--------------------------------------------------------------------------------
1. Chaining Decorators
2. Decorator Order
3. Stateful Decorators
4. Decorators with State
5. Timer Decorators
6. Logging Decorators
7. Validation Decorators
8. Retry Decorators
9. Caching Decorators
10. Authentication Decorators

Chapter 111: Method Decorators
--------------------------------------------------------------------------------
1. @staticmethod
2. @classmethod
3. @property
4. Property Setters and Deleters
5. Method Decorators vs. Function Decorators
6. Decorating Instance Methods
7. `self` and `cls` in Decorators
8. Abstract Method Decorators

Chapter 112: The `operator` Module
--------------------------------------------------------------------------------
1. Introduction to `operator`
2. Arithmetic Operators
3. Comparison Operators
4. Logical Operators
5. `operator.itemgetter`
6. `operator.attrgetter`
7. `operator.methodcaller`
8. Using `operator` with `functools`

Chapter 113: Functional Programming Best Practices
--------------------------------------------------------------------------------
1. Pure Function Design
2. Avoiding Side Effects
3. Immutable Data Structures
4. Function Composition Strategies
5. When to Use Functional Patterns
6. Readability vs. Cleverness
7. Performance Considerations
8. Debugging Functional Code
9. Testing Functional Code
10. Combining Paradigms


APPENDICES
================================================================================

- Built-in Functions
- Comments and Documentation Conventions
- Common Idioms
- Common String Methods
- Error Handling Basics
- Input Validation Patterns
- Naming Conventions
- Operator Precedence


END OF INDEX
================================================================================

This index represents a comprehensive Python 3 programming curriculum
integrating fundamental concepts with object-oriented programming principles,
advanced polymorphism, abstract programming patterns, data structures mastery,
modules and packages, file I/O operations, robust error handling techniques,
professional development environment management, data validation with Pydantic,
and functional programming paradigms.

Topics are organised logically by concept, not by assignment order.
Combines content from Growing Code, CodeCultivation, Garden Guardian,
Data Quest, Data Archivist, Code Nexus, The Codex, DataDeck, The Matrix,
Cosmic Data, and FuncMage assignments.

Structure: 1.3. Parts | 1.1.3. Chapters | 3. Appendices

**Key Learning Progression:**
- Fundamentals → Functions → OOP Basics → Advanced OOP & Polymorphism
- Abstract Programming & Interfaces → Data Structures → Modules & Packages
- File I/O → Exception Handling → Design Principles & Best Practices
- Development Environment & Professional Tools → Data Validation & Serialization
- Functional Programming Paradigms

Complete Coverage:
- Python syntax and semantics
- Variables, data types, operators
- Control flow (conditionals, loops, recursion)
- Functions and type hints
- Object-oriented programming (classes, inheritance, encapsulation)
- Advanced OOP (polymorphism, ABC, protocols, method overriding)
- Abstract programming (interfaces, multiple inheritance, design patterns)
- Abstract Factory and Strategy patterns
- Interface composition and multiple interface implementation
- Data structures (lists, tuples, sets, dictionaries)
- Generators and comprehensions
- Modules and packages (imports, __init__.py, absolute/relative imports)
- Circular dependency resolution
- File I/O and streams
- Exception handling and error management
- SOLID principles and design patterns
- Testing and debugging strategies
- Virtual environments (venv)
- Package management (pip, Poetry)
- Dependency management and requirements files
- Environment variables and configuration
- Security and secrets management
- Professional Python project structure
- Development workflow and best practices
- Data validation with Pydantic
- BaseModel and Field validation
- Custom validation with @model_validator
- Nested models and complex relationships
- Enums and type validation
- Serialisation and deserialization
- ValidationError handling
- Functional programming paradigms
- Lambda expressions and anonymous functions
- Higher-order functions
- Map, filter, and reduce
- Closures and lexical scoping
- The functools module (reduce, partial, lru_cache, wraps)
- Decorators (basic and advanced)
- Parameterised decorators and decorator factories
- Method decorators (@staticmethod, @classmethod, @property)
- The operator module
- Pure functions and immutability
