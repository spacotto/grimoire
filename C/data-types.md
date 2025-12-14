# C Language Data Types Documentation
A **data type** in C is a keyword that tells the compiler **how much memory** to allocate for a variable and **what kind of data** (like integers, characters, or floating-point numbers) it will store. This definition determines the **range of values** a variable can hold and the **operations** that can be performed on it.

>[!IMPORTANT]
>Actual sizes and ranges depend on the system architecture. Use `<limits.h>` and `<float.h>` headers for precise minimum and maximum values on your platform.

## Basic Data Types
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

## Type Qualifiers
### `const`

### `volatile`

### `restrict`

## Size and Range
Use `sizeof()` operator to get the size of any type in bytes:
```
(char *)malloc(sizeof(char) * 42)
```
