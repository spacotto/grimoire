# Recursion
Recursion is a technique where a function calls itself to solve a problem by breaking it into smaller subproblems. It's a powerful alternative to iteration, but requires careful design to avoid infinite loops and stack overflows.

## What is Recursion?

A **recursive function** is one that calls itself within its own body.

```python
def countdown(n):
    if n <= 0:
        print("Done!")
    else:
        print(n)
        countdown(n - 1)  # recursive call
```

>[!NOTE]
>Each call creates a new frame on the **call stack**. Python's default recursion limit is **1000** (`sys.getrecursionlimit()`).

## Base Cases and Recursive Cases

Every recursive function needs:

- **Base case** — the condition that stops the recursion.
- **Recursive case** — the part that calls the function again with a smaller input.

```python
def factorial(n):
    if n == 0:          # base case
        return 1
    return n * factorial(n - 1)  # recursive case
```

>[!WARNING]
>Without a base case, the function recurses infinitely and raises `RecursionError`.

## Helper Functions for Recursion

When a recursive function needs extra state (e.g., an accumulator), use a **helper function** to keep the public API clean.

```python
def sum_list(lst):
    def helper(lst, acc):
        if not lst:           # base case
            return acc
        return helper(lst[1:], acc + lst[0])
    return helper(lst, 0)
```

>[!NOTE]
>This pattern avoids exposing internal parameters to the caller.

## Default Parameter Values in Recursion

An alternative to helper functions — use **default parameters** to carry state.

```python
def sum_list(lst, acc=0):
    if not lst:
        return acc
    return sum_list(lst[1:], acc + lst[0])
```

>[!CAUTION]
>Avoid mutable defaults (e.g., `acc=[]`) — they persist between calls.

## Iteration vs. Recursion

| | Iteration | Recursion |
|---|---|---|
| **Speed** | Generally faster | Slower (call stack overhead) |
| **Memory** | Constant stack | Stack grows with depth |
| **Readability** | Verbose for tree/graph problems | Elegant for naturally recursive problems |
| **Risk** | None | `RecursionError` if depth > limit |

```python
# Iterative factorial
def factorial_iter(n):
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result

# Recursive factorial
def factorial_rec(n):
    if n == 0:
        return 1
    return n * factorial_rec(n - 1)
```

>[!WARNING]
>Python does **not** optimise tail recursion, so deep recursion will hit the stack limit.

## When to Use Recursion

**Use recursion when:**
- The problem has a naturally recursive structure (trees, graphs, nested data).
- The solution is significantly cleaner than the iterative version.
- Input depth is bounded and won't exceed Python's recursion limit.

**Avoid recursion when:**
- Working with large inputs (risk of `RecursionError`).
- Performance is critical (function call overhead matters).
- An iterative solution is equally readable.

**Common use cases:** binary search, tree traversal, merge sort, parsing nested structures, backtracking algorithms.
