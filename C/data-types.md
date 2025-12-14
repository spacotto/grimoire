# C Language Data Types Documentation
A **data type** in C is a keyword that tells the compiler **how much memory** to allocate for a variable and **what kind of data** (like integers, characters, or floating-point numbers) it will store. This definition determines the **range of values** a variable can hold and the **operations** that can be performed on it.

>[!IMPORTANT]
>Actual sizes and ranges depend on the system architecture. Use `<limits.h>` and `<float.h>` headers for precise minimum and maximum values on your platform.

>[!TIP]
>Use `sizeof()` operator to get the size of any type in bytes:
>```
>(char *)malloc(sizeof(char) * 42)
>```

## Type Qualifiers
**Type Qualifiers** are keywords that change how the compiler treats a variable in memory or optimisation, but they do not change the data's range or size.

#### `const`
>It makes a variable read-only.

#### `volatile`
>It tells the compiler the value may change unexpectedly.

#### `restrict`
>It tells the compiler that for the lifetime of the pointer, no other pointer will be used to access the object to which it points. It can be used in pointer declarations to make optimisations (for example, vectorisation)

## Type Specifiers (or Modifiers)
**Type Specifiers** (or **Modifiers**) are keywords that define the variable's underlying data type and change its size and/or range of values.

### Primary Specifiers
Data types that are used to store simple values.

#### `void`
>Lorem ipsum.

#### `char`
>It is used to store a single character (like ASCII).

>[!NOTE]
>Since it is fundamentally an integer type, it stores the character's corresponding numerical value, usually following the ASCII standard.

#### `int`
>It is used to store whole numbers (integers). It handles general-purpose counting and arithmetic.

>[!TIP]
>On 32-bit systems, its range goes from -2147483648 (`INT_MIN`) to 2147483647 (`INT_MAX`). This range can be extended or reduced using modifiers like `long` or `short` (see below).

#### `float`
>Lorem ipsum.

### Sign Specifiers
Data types that define the sign (negative or positive) of another data type.

#### `signed`
>Lorem ipsum.

#### `unsigned`
>Lorem ipsum.

### Size or Range Specifiers
Data types that modify the size or range of another data type.

#### `short`
>Lorem ipsum.

#### `long`
>Lorem ipsum.

#### `long long`
>Lorem ipsum.

### Derived Types
Data types that are built from the basic types.

#### Pointers
>Lorem ipsum.

#### Arrays & Strings
>Lorem ipsum.

### User-Defined Types
Data types that are defined by the user.

#### Structures
>Lorem ipsum.

#### Unions
>Lorem ipsum.

#### Enumerations
>Lorem ipsum.
