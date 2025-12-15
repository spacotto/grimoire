# Using Temporary Variables with Pointers to Save Values in C

When working with pointers in C, it's common to encounter situations where you need to modify multiple values from a function. But what if those values depend on each other, and changing one too early would overwrite important information?
That’s where **temporary variables** come in. Temporary variables are used to **store intermediate values**, so you don’t lose important data during computation.

## Implementation Example
```
void	ft_swap(int *a, int *b)
{
	int	tmp;

	tmp = *a;  \\ We need to store the content of a before moving the content of b to avoid losing data
	*a = *b;
	*b = tmp;
}

#include <stdio.h>
int main(void)
{
	int a = 33;
	int b = 42; 

	printf("=== BEFORE SWAP\n[A] Adress: %p | Content: %d\n[B] Adress: %p | Content: %d\n\n", &a, a, &b, b);
	ft_swap(&a, &b);
	printf("=== AFTER SWAP\n[A] Adress: %p | Content: %d\n[B] Adress: %p | Content: %d\n", &a, a, &b, b);
	return(0);
}
```
