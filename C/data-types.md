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

`const`
>It makes a variable read-only.

`volatile`
>It tells the compiler the value may change unexpectedly.

`restrict`
>It tells the compiler that for the lifetime of the pointer, no other pointer will be used to access the object to which it points. It can be used in pointer declarations to make optimisations (for example, vectorisation)

## Type Specifiers (or Modifiers)
**Type Specifiers** (or **Modifiers**) are keywords that define the variable's underlying data type and change its size and/or range of values.

### Basic Specifiers

### Sign Specifiers

### Size or Range Specifiers

### Derived Types





