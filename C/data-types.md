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

### `const`
`const` makes a variable read-only.

### `volatile`
`volatile` tells the compiler the value may change unexpectedly. It is used in embedded systems and multi-threading.

### `restrict`
`restrict` can be used in pointer declarations. By adding this type qualifier, a programmer hints to the compiler that for the lifetime of the pointer, no other pointer will be used to access the object to which it points. This allows the compiler to make optimisations (for example, vectorisation) that would not otherwise have been possible.

## Type Specifiers (or Modifiers)
**Type Specifiers** (or **Modifiers**) are keywords that define the variable's underlying data type and change its size and/or range of values.

### Integer Types
#### `int`
```
int           # Standard integer type, typically 4 bytes (32 bits)
```

It is the most frequently used basic data type and is designed to store **whole numbers** (both positive and negative) without any fractional or decimal components (e.g., `42`).

#### `short`
```
short          # Short integer, typically 2 bytes
```

#### `long`
```
long           # Long integer, typically 4 or 8 bytes
```

#### `long long`
```
long long      # Extended integer, at least 8 bytes
```

### Character Types

### Floating-Point Types

### Void Type

## Derived Types

### Arrays

### Pointers

### Structures

### Unions

### Enumerations
