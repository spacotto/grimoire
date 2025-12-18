# About Linked Lists
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

## Key Points
- Always check for `NULL` pointers before dereferencing
- Use double pointers (`**`) when modifying the head pointer
- Always free allocated memory to prevent memory leaks
- The head pointer is your only entry point to the list

# Linked Lists Syntax
### Basic Node Structure
```
typedef struct s_list
{
	void			*content;        // The value stored in the node
	struct s_list	*next;           // Pointer to the next node
}	t_list;
```

## Creation
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

## Addition
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

## Travelling
### Travelling to the End
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

## Searching
### Searching Content
```
t_list	*ft_lstchr(t_list *haystack, int needle)
{
	t_list	*location;

    if (!haystack || !needle)
		return (NULL);
	location = haystack;
    while (location != NULL)
	{
        if (location->content == needle)
            return (location);
        location = location->next;
    }
    return (NULL);
}
```

## Sorting
### Sorting Content
```
int ascending(int a, int b)
{
	return (a <= b);
}

t_list	*ft_sort_list(t_list* lst, int (*cmp)(int, int))
{
	t_list	sorted;

	return (sorted);
}
```

## Deleting
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
		del(lst->content);
	free(lst);
}
```

### Deleting the List
```
void	del(void *data)
{
	if (data)
		free(data);
	else
		return ;
}

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
