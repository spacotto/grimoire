# About Bitwise Operations
Bitwise operations **manipulate individual bits in integer types**. They work on the **binary representation of numbers** and are fundamental for low-level programming, hardware control, and performance optimisation.

## Bitwise Operators
### AND (`&`)
Sets each bit to 1 if both bits are 1
```
5 & 3        // 0101 & 0011 = 0001 (result: 1)
```

### OR (`|`)
Sets each bit to 1 if at least one bit is 1
```
5 | 3        // 0101 | 0011 = 0111 (result: 7)
```

### XOR (`^`)
Sets each bit to 1 if bits are different
```
5 ^ 3        // 0101 ^ 0011 = 0110 (result: 6)
```

### NOT (`~`)
Inverts all bits (unary operator)
```
~5           // ~0101 = 1010 (result depends on integer size)
```

### Left Shift (`<<`)
Shifts bits left, filling with zeros
```
5 << 2       // 0101 << 2 = 10100 (result: 20)
```

### Right Shift (`>>`)
Shifts bits right
```
5 >> 1       // 0101 >> 1 = 0010 (result: 2)
```

## Common Use Cases
- Setting a bit: `num |= (1 << n)` sets the nth bit to 1
- Clearing a bit: `num &= ~(1 << n)` sets the nth bit to 0
- Toggling a bit: `num ^= (1 << n)` flips the nth bit
- Checking a bit: `(num & (1 << n)) != 0` tests if nth bit is 1
- Creating masks: Use combinations to isolate specific bits
```
unsigned char lower_nibble = value & 0x0F;  // Keep lower 4 bits
unsigned char upper_nibble = value & 0xF0;  // Keep upper 4 bits
```

## Important Notes
Bitwise operations work on integer types (char, short, int, long). The right shift behaviour for signed integers is implementation-defined—it may perform arithmetic shift (preserving sign bit) or logical shift (filling with zeros). For unsigned integers, right shift always fills with zeros.
Avoid shifting by a negative amount or by more bits than the type width—this causes undefined behaviour.

Practical Example:
```
#define FLAG_READ  (1 << 0)               // 0001
#define FLAG_WRITE (1 << 1)               // 0010
#define FLAG_EXEC  (1 << 2)               // 0100

unsigned char permissions = 0;
permissions |= FLAG_READ | FLAG_WRITE;    // Set read and write

if (permissions & FLAG_WRITE) {
    // Write permission is set
}

permissions &= ~FLAG_WRITE;               // Remove write permission
```
