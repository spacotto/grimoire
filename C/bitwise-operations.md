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

## Common Bitwise Syntax Patterns
Creating a bit mask: 
```
(1 << n)				\\ Creates a mask with only the nth bit set
```

Setting a bit: 
```
num |= (1 << n) 		\\ Sets the nth bit to 1
```

## Examples
### Print Bits
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

### Reverse Bits
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
1. Initialise the counter `i` to 0.
2. Initialise the byte `result` where the reversed bits will be built.
3. Loop through all 8 positions (from `i = 0` to `i = 7`).
	- Check the bit in the byte (octet).
	- `(1 << i)` creates a mask like `00000001`, `00000010`, `00000100`, etc.
 	- The condition is TRUE if the i-th bit of `octet` is `1`.
4. Set the corresponding reversed bit in the `result`.
	- `(1 << (7 - i))` creates a reverse mask:
		- When `i = 0` (checking the LSB), (`7 - i`) is 7 (setting the MSB). Mask: `10000000`
  		- When `i = 1`, (`7 - i`) is 6. Mask: `01000000`
    	- When `i = 7` (checking the MSB), (7-i) is 0 (setting the LSB). Mask: `00000001`
     	- `result |= ...` uses the bitwise OR (`|`) assignment to set that specific bit in 'result'.

### Swap Bits
```
unsigned char	swap_bits(unsigned char octet)
{
	return ((octet & 0x0F) << 4) | (octet >> 4);
}
```

### Power of 2?
```
int	is_power_of_2(unsigned int n)
{
	return ((n > 0) && ((n & (n - 1)) == 0));
}
```
>[!NOTE]
>A number n is a power of 2 if and only if:
>1. n is greater than 0 (powers of 2 are positive, aka `n > 0`)
>2. The bitwise AND (`&`) of n and `(n - 1)` is 0.
>
>`n & (n - 1)` clears the least significant set bit (the rightmost 1-bit) in n.
>If n is a power of 2, it has only one set bit, so clearing it results in 0.

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
