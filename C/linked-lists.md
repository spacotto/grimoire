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

## Types of Linked Lists
- **Singly Linked List.** Each node points to the next node. The last node points to `NULL`.
- **Doubly Linked List.** Each node has pointers to both next and previous nodes.
- **Circular Linked List.** The last node points back to the first node instead of `NULL`.
