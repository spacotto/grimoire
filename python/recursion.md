# About Recursion (Python)
**Recursion** is when a function **calls itself with modified parameters** to solve a problem by breaking it into smaller instances of the same problem. **The `range()` function** generates a sequence of numbers, commonly used with loops.

**Key concepts:**
- Recursion requires a base case to stop the function from calling itself infinitely (**exit condition**)
- Helper functions can be defined inside or outside the main function for recursion

## Approaches to Recursion (Examples)
### Nested helper function
```python
def ft_count_harvest_recursive():
    # Exit condition provided as input
    limit = int(input("Days until harvest: "))
    
    # Recursive helper function
    def helper(current):
        # Exit condition
        if current > limit:
            return
        
        # Print the current state
        print("Day", current)
        
        # Helper function call
        helper(current + 1)
    
    # Init at Day 1
    helper(1)
    print("Harvest time!")
```

### Separate helper function
```python
# Helper function
def harvest_helper(current, limit):
    if current > limit:                 # Exit condition
        return
    print("Day", current)
    harvest_helper(current + 1, limit)  # Helper function call (triggers recursivity)

# Core function
def ft_count_harvest_recursive():
    limit = int(input("Days until harvest: "))
    harvest_helper(1, limit)            # Helper function call            
    print("Harvest time!")
```

### Default parameter values
```python
def ft_count_harvest_recursive(current=1, limit=None):
    # Initialisation: Only happens on the very first call
    if limit is None:
        limit = int(input("Days until harvest: "))
    
    # Exit condition
    if current > limit:
        print("Harvest time!")
        return

    # Print the current day
    print("Day", current)
    
    # Recursion: 'current' becomes 'current + 1', 'limit' stays the same
    ft_count_harvest_recursive(current + 1, limit)
```

| Approach | Best for... | Why? |
| :--- | :--- | :--- |
| Nested Helper | One-off tasks | Keeps the helper function hidden inside the main function so it can't be called by mistake from elsewhere in your code. |
| Separate Helper | Reusability | Makes the code easier to test and read by separating the "setup" (getting input) from the "logic" (the counting). |
| Default Parameters | Clean, concise code | It’s the most "Pythonic" way. It allows the function to be flexible: it can be called without arguments or with a specific starting point. |
