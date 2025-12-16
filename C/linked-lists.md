# Linked Lists
A linked list is a dynamic data structure where elements (**nodes**) are **connected** through **pointers**. Unlike arrays, linked lists **don't require contiguous memory** and can **grow or shrink during runtime**.

## Characteristics
- **Dynamic structure.** Nodes connected through pointers, not stored contiguously in memory like arrays
- **Node composition.** Each node contains data and a pointer to the next node; the last node points to NULL
- **Flexible memory.** No pre-allocated fixed size required; grows and shrinks at runtime by allocating/freeing individual nodes
- **Sequential access.** Must traverse from the head to reach elements, resulting in O(n) access time (no direct indexing)
- **Efficient insertions/deletions.** Adding or removing nodes only requires pointer manipulation, not shifting elements
- **Memory overhead.** Uses extra memory to store pointers alongside data
- **Best use cases.** Ideal when frequent insertions/deletions are needed and random access isn't a priority

## Types of Linked Lists
- **Singly Linked List.** Each node points to the next node. The last node points to `NULL`.
- **Doubly Linked List.** Each node has pointers to both next and previous nodes.
- **Circular Linked List.** The last node points back to the first node instead of `NULL`.

## Basic Node Structure
```
typedef struct s_list
{
	void			*content;        // The value stored in the node
	struct s_list	*next;           // Pointer to the next node
}	t_list;
```

## Essential Operations
### Creating a Node
```
t_list	*ft_lstadd_node(void *content)
{
	t_list	*node;

	node = ft_calloc(1, sizeof(t_list));
	if (!node)
		return (NULL);
	node->content = content;
	node->next = NULL;
	return (node);
}
```

### Adding at the Beginning
```
void	ft_lstadd_front(t_list **lst, t_list *new)
{
	if (!new)
		return ;
	new->next = *lst;
	*lst = new;
}
```

### Measuring the List
```
int	ft_lstsize(t_list *lst)
{
	int	size;

	size = 0;
	while (lst)
	{
		size++;
		lst = lst->next;
	}
	return (size);
}
```

### Travelling the List
```
t_list	*ft_lstlast(t_list *lst)
{
	if (!lst)
		return (NULL);
	while (lst && lst->next)
		lst = lst->next;
	return (lst);
}
```

### Adding at the End
```
void	ft_lstadd_back(t_list **lst, t_list *new)
{
	t_list	*last;

	if (!lst || !new)
		return ;
	if (*lst == NULL)
		*lst = new;
	else
	{
		last = ft_lstlast(*lst);
		last->next = new;
	}
}
```

### Searching Content
```
int	ft_lst_search(t_list *lst, int key)
{
    if (!lst)
		return (NULL);
	struct Node* temp = head;
    while (temp != NULL)
	{
        if (temp->data == key)
            return (1);  // Found
        temp = temp->next;
    }
    return (0);  // Not found
}
```

### Deleting a Node
```
void    del(void *data)
{
	if (data)
		free(data);
	else
		return ;
}

void	ft_lstdelone(t_list *lst, void (*del)(void*))
{
	if (!lst || !del)
		return ;
	else
	{
		if (data)
			free(data);
		else
			return ;
	}
	free(lst);
}
```

### Deleting the List
```
void	ft_lstclear(t_list **lst, void (*del)(void*))
{
	t_list	*head;
	t_list	*node;

	if (!lst || !del)
		return ;
	head = *lst;
	while (head)
	{
		node = head->next;
		ft_lstdelone(head, del);
		head = node;
	}
	*lst = NULL;
}
```

### Traversing and Printing

## Key Points
- Always check for `NULL` pointers before dereferencing
- Use double pointers (`**`) when modifying the head pointer
- Always free allocated memory to prevent memory leaks
- The head pointer is your only entry point to the list
