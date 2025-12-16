# Linked Lists
A linked list is a dynamic data structure where elements (**nodes**) are **connected** through **pointers**. Unlike arrays, linked lists **don't require contiguous memory** and can **grow or shrink during runtime**.

## Basic Node Structure
```
typedef struct s_list
{
	void			*content;        // The value stored in the node
	struct s_list	*next;           // Pointer to the next node
}	t_list;
```
