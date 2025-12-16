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
- Bitwise operations work on integer types (`char`, `short`, `int`, `long`).
- The right shift behaviour for signed integers is implementation-defined. It may perform an arithmetic shift (preserving the sign bit) or logical shift (filling with zeros).
- For unsigned integers, right shift always fills with zeros.
- Avoid shifting by a negative amount or by more bits than the type width. This causes undefined behaviour.
- The **octet** is a unit of digital information in computing and telecommunications that consists of **8 bits**. The term is often used when **the term byte might be ambiguous**, as the term byte has historically been used for storage units of a variety of sizes.
```
0000 0010				// This is how an octet looks like
```
- A **(bit)mask** is a binary value used to selectively manipulate specific bits within another binary value through bitwise operations.

## Print Bits
You can extract and display each bit of a byte by iterating through each bit position. 
```
#include <unistd.h>

void	print_bits(unsigned char octet)			// The fn takes an unsigned char for its size is 1 byte (1 octet, 8 bits)
{
	int	            i;
	unsigned char	bit;

	i = 7;										// i is initialised at 7 since we start from 0 for a total of 8
	while (i >= 0)
	{
		bit = (octet & (1 << i)) ? '1' : '0';	// The mask checks each bit from position 7 to position 0
		write(1, &bit, 1);
		i--;
	}
}
```

## Reverse Bits
```
unsigned char	reverse_bits(unsigned char octet)
{
	int				i;
	unsigned char	result;

	i = 0;
	result = 0;
	while (i < 8)						\\ Check until the 7th position
	{
		if (octet & (1 << i)) 			\\ Check each bit in the original byte
			result |= (1 << (7 - i));	\\ Place it in the corresponding reversed position in a result byte
		i++;
	}
	return (result);
}
```
