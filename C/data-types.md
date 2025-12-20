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

| Qualifiers | Purpose                                                                                                                            |
| :--------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| `const`    | It makes a variable read-only.                                                                                                     |
| `volatile` | It tells the compiler the value may change unexpectedly.                                                                           |
| `restrict` | It tells the compiler that for the lifetime of the pointer, no other pointer will be used to access the object to which it points. |

>[!NOTE]
> `restrict` can be used in pointer declarations to make optimisations (for example, vectorisation)

## Type Specifiers (or Modifiers)
**Type Specifiers** (or **Modifiers**) are keywords that define the variable's underlying data type and change its size and/or range of values.

### Primary Specifiers
Data types that are used to store simple values.

| Specifiers | Purpose                                                                                          | Size (typical) |
| :--------- | :----------------------------------------------------------------------------------------------- | :------------- |
| `void`     | Represents the absence of type. Used for functions that return no value or generic pointers.     | 0 bytes        |
| `char`     | Stores a single character (ASCII/UTF-8). Used for text and small integer values.                 | 1 bytes        |
| `int`      | Stores whole numbers (integers). Used for general-purpose counting and arithmetic operations.    | 4 bytes        |
| `float`    | Stores single-precision floating-point numbers. Used for decimal values with moderate precision. | 4 bytes        |

>[!NOTE]
>Since `char` is fundamentally an integer type, it stores the character's corresponding numerical value, usually following the ASCII standard.

>[!TIP]
>On 32-bit systems, `int` range goes from -2147483648 (`INT_MIN`) to 2147483647 (`INT_MAX`). This range can be extended or reduced using modifiers like `long` or `short` (see below).

### Sign Specifiers
Data types that define the sign (negative or positive) of another data type.

| Specifiers | Purpose                                              | Effect on Range |
| :--------- | :--------------------------------------------------- | :-------------- |
| `signed`   | Indicates the data type can store both negative and positive values. This is the default for `int` and `char` on most systems. | Uses one bit for sign, reducing positive range but allowing negatives. |
| `unsigned` | Indicates the data type can store only positive values (including zero). Eliminates the sign bit to double the positive range. | All bits used for magnitude, doubling the maximum positive value. |

### Size or Range Specifiers
Data types that modify the size or range of another data type.

| Specifiers  | Purpose                                              |
| :---------- | :--------------------------------------------------- |
| `short`     |  |
| `long`      |  |
| `long long` |  |

### Derived Types
Data types that are built from the basic types.

| Specifiers | Purpose                                              |
| :--------- | :--------------------------------------------------- |
| Pointers   |  |
| Arrays     |  |

### User-Defined Types
Data types that are defined by the user.

| Specifiers   | Purpose                                              |
| :----------- | :--------------------------------------------------- |
| Structures   |  |
| Unions       |  |
| Enumerations |  |



