# About Bitwise Operations
Bitwise operations **manipulate individual bits in integer types**. They work on the **binary representation of numbers** and are fundamental for low-level programming, hardware control, and performance optimisation.

>[!NOTE]
>Bitwise operations work on integer types (`char`, `short`, `int`, `long`).

## Useful Terms
### Mask
A **(bit)mask** is a binary value used to selectively manipulate specific bits within another binary value through bitwise operations.

### Octet
A unit of digital information in computing and telecommunications that consists of **8 bits**. The term is often used when **the term byte might be ambiguous**, as the term byte has historically been used for storage units of a variety of sizes.
```
0000 0010				// This is how an octet looks like
```

>[!TIP]
>Use a **Programmer Calculator** to visualise bytes and bits.

# Bitwise Operators
Bitwise operations manipulate individual bits within integers using specific operators.

## AND (`&`)
Sets each bit to 1 if both bits are 1
```
5 & 3        // 0101 & 0011 = 0001 (result: 1)
```

## OR (`|`)
Sets each bit to 1 if at least one bit is 1
```
5 | 3        // 0101 | 0011 = 0111 (result: 7)
```

## XOR (`^`)
Sets each bit to 1 if bits are different
```
5 ^ 3        // 0101 ^ 0011 = 0110 (result: 6)
```

## NOT (`~`)
Inverts all bits (unary operator)
```
~5           // ~0101 = 1010 (result depends on integer size)
```

## Left Shift (`<<`)
Shifts bits left, filling with zeros
```
5 << 2       // 0101 << 2 = 10100 (result: 20)
```

## Right Shift (`>>`)
Shifts bits right
```
5 >> 1       // 0101 >> 1 = 0010 (result: 2)
```

>[!IMPORTANT]
>The right shift behaviour for `int` is implementation-defined: it may perform an arithmetic shift (preserving the sign bit) or logical shift (filling with zeros). For `unsigned int`, right shift always fills with zeros.

>[!CAUTION]
>Avoid shifting by a negative amount or by more bits than the type width. This causes undefined behaviour.

# Common Bitwise Syntax Patterns
Common bitwise syntax patterns include the use of operators such as AND (`&`), OR (`∣`), XOR (`^`), NOT (`~`), left shift (`<<`), right shift (`>>`), and zero-fill right shift (`>>>`). These operators are used to manipulate individual bits within integers, enabling efficient operations like setting, clearing, toggling, or testing specific bits.
 
## Bit Manipulation Fundamentals
Creating a bit mask: 
```
(1 << n)											// Creates a mask with only the nth bit set
```

Setting a bit: 
```
num |= (1 << n) 									// Sets the nth bit to 1
```

Clearing a bit: 
```
num &= ~(1 << n) 									// Sets the nth bit to 0
```

Toggling a bit: 
```
num ^= (1 << n) 									// Flips the nth bit
```

Checking a bit: 
```
(num & (1 << n))									// Isolates the nth bit (non-zero if set)
```

Testing if bit is set: 
```
(num & (1 << n)) != 0 								// Explicitly tests for 1
```

### Masking Operations
Isolating lower bits: 
```
num & 0x0F 											// Keeps only the lower 4 bits (nibble)
```

Isolating upper bits: 
```
num & 0xF0 											// Keeps only the upper 4 bits
```

Extracting bit range: Combine shift and mask operations
```
unsigned char middle = (octet >> 2) & 0x07;			// Extract bits 2-4
```

## Shift Operations
Multiply by power of 2: 
```
num << n multiplies by 2^n
```

Divide by power of 2: 
```
num >> n divides by 2^n (for unsigned)
```

Moving bits to position: Shift then mask or mask then shift
```
(value & 0x01) << 7;  // Move bit 0 to bit 7
(value >> 5) & 0x01;  // Move bit 5 to bit 0
```

## Combining Operations
Swap nibbles:
```
((num & 0x0F) << 4) | (num >> 4)
```

Clear then set: 
```
num = (num & ~mask) | new_bits
```

Multiple bits at once: Use combined masks
```
octet |= (1 << 2) | (1 << 5);   // Set bits 2 and 5
octet &= ~((1 << 1) | (1 << 3)); // Clear bits 1 and 3
```

# Examples
## Power of 2?
```
int	is_power_of_2(unsigned int n)
{
	return ((n > 0) && ((n & (n - 1)) == 0));
}
```

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
    int             i;
    unsigned char   result;

	i = 0;
	result = 0;
	while (i < 8)
	{
		if (octet & (1 << i))
			result |= (1 << (7 - i));
		i++;
	}
	return (result);
}
```

## Swap Bits
```
unsigned char	swap_bits(unsigned char octet)
{
	return ((octet & 0x0F) << 4) | (octet >> 4);
}
```
