# Multithreading in C

Multithreading allows a program to run multiple tasks concurrently within a single process. In C, this is achieved via the **POSIX Threads (pthreads)** library. It improves performance on multi-core systems but introduces complexity around shared memory and synchronisation.

## What is Multithreading?

A **thread** is the smallest unit of execution within a process. All threads in a process share the same memory space (heap, globals) but have their own **stack** and **registers**.

**Process vs Thread:**

| | Process | Thread |
|---|---|---|
| **Memory** | Separate | Shared |
| **Creation cost** | High | Low |
| **Communication** | IPC needed | Direct (shared memory) |
| **Isolation** | Strong | Weak |

**Basic thread lifecycle:**
1. Created → 2. Running → 3. Blocked (waiting) → 4. Terminated

## Advantages & Disadvantages

**Advantages:**
- Better CPU utilisation on multi-core hardware
- Faster than spawning processes (lower overhead)
- Shared memory = efficient data exchange between threads
- Keeps programs responsive (e.g., UI thread + worker thread)

**Disadvantages:**
- Hard to debug (non-deterministic execution order)
- Risk of **race conditions**, **deadlocks**, and **starvation**
- Shared memory requires careful synchronisation
- More complex code = more maintenance burden

## Implementation in C: Thread Synchronisation Mechanisms

Compile with: `cc file.c -lpthread`

### `pthread_create`

`pthread_create()` spawns a new thread that executes `start_routine(arg)`.

```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
                   void *(*start_routine)(void *), void *arg);
```

| Parameter | Description |
| :--- | :--- |
| `thread` | Pointer to store the new thread's ID |
| `attr` | Thread attributes (stack size, scheduling…); `NULL` = defaults |
| `start_routine` | Function the thread runs — signature: `void *fn(void *)` |
| `arg` | Argument passed to `start_routine`; cast to/from `void *` |

Returns `0` on success, an error code otherwise. The new thread runs concurrently with the caller immediately after creation.

### `pthread_join`

Blocks the calling thread until `thread` terminates.

```c
int pthread_join(pthread_t thread, void **retval);
```

| Parameter | Description | 
| :--- | :--- |
| `thread` | ID of the thread to wait for |
| `retval` | If non-`NULL`, receives the value returned by `start_routine` |

>[!WARNING]
>A thread that is never joined is a **resource leak** (its stack and descriptor persist). Always join, or detach with `pthread_detach()`.

| Function | Purpose |
| :--- | :--- |
| `pthread_exit()` | Terminates calling thread (passes return value to `pthread_join`) |
| `pthread_self()` | Returns the calling thread's own ID |

### Mutex

>[!IMPORTANT]
>Always unlock what you lock. A missing unlock causes a **deadlock**.
>[!IMPORTANT]
>Always use a `while` loop (not `if`) to recheck the condition after waking—**spurious wakeups** can occur.

## Concurrency Issues

### Race Condition
Two threads read/write shared data simultaneously → unpredictable result.

```c
// BAD: counter++ is not atomic (read → modify → write)
counter++;

// FIX: wrap in mutex lock/unlock
```

### Deadlock
Two threads each hold a lock; the other needs → both wait forever.

```c
// Thread A          // Thread B
lock(mutex1);        lock(mutex2);
lock(mutex2);        lock(mutex1);  // DEADLOCK
```

**Prevention rules:**
- Always acquire locks in the **same order**
- Use `pthread_mutex_trylock()` to avoid blocking indefinitely
- Minimize the number of locks held simultaneously

### Starvation
A thread is perpetually denied access to a resource because others keep taking priority.

**Fix:** Use fair scheduling policies or bounded waiting mechanisms.

### Priority Inversion
A high-priority thread is blocked by a low-priority thread holding a needed lock.

**Fix:** Use **priority inheritance** (OS-level) or redesign lock usage.

### False Sharing
Two threads modify **different variables** that share the same CPU cache line → performance hit from constant cache invalidation.

**Fix:** Pad data structures so each thread's data occupies a separate cache line.

```c
// Pad to cache line size (typically 64 bytes)
typedef struct {
    long value;
    char padding[56];
} padded_counter_t;
```

## Quick Reference

| Mechanism | Use case |
|---|---|
| `pthread_mutex_t` | Protect a critical section |
| `sem_t` | Limit concurrent access (N threads) |
| `pthread_cond_t` | Wait for a condition to be true |
| `pthread_rwlock_t` | Many readers, one writer |

*POSIX Threads — compile with `-lpthread`*
