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

### Creating & Joining Threads

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

---

`pthread_join()` Blocks the calling thread until `thread` terminates.

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

### Mutex (Mutual Exclusion)

Mutex functions **prevent two threads from accessing a critical section simultaneously**.

```c
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;

void *safe_increment(void *arg) {
    pthread_mutex_lock(&lock);
    // --- critical section ---
    counter++;
    // --- end critical section ---
    pthread_mutex_unlock(&lock);
    return NULL;
}
```

>[!IMPORTANT]
>Always unlock what you lock. A missing unlock causes a **deadlock**.

---

`pthread_mutex_init()` initialises a mutex dynamically. Use this when the mutex is heap-allocated or needs custom attributes (e.g. recursive locking). For static/global mutexes, prefer the macro `PTHREAD_MUTEX_INITIALIZER` instead.

```c
int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutexattr_t *attr);
```

| Parameter | Description |
| :--- | :--- |
| `mutex` | Pointer to the mutex to initialise |
| `attr` | Mutex attributes; `NULL` = default (non-recursive, non-robust) |

---

`pthread_mutex_lock` acquires the mutex. If another thread already holds it, it blocks until it is released. Only one thread can hold a given mutex at a time—this is the core guarantee that protects the critical section.

```c
int pthread_mutex_lock(pthread_mutex_t *mutex);
```

>[!WARNING]
>Calling `pthread_mutex_lock` on a mutex you already hold causes a deadlock (with a default non-recursive mutex).

---

`pthread_mutex_unlock` releases the mutex, allowing one waiting thread to acquire it. Must be called by the same thread that locked it. Always pair every lock with an unlock — even on error paths.

```c
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```

---

`pthread_mutex_destroy` frees resources associated with a mutex. Only call on an unlocked mutex with no threads waiting on it. Required for dynamically initialised mutexes to avoid resource leaks; no-op for statically initialised ones.

```c
int pthread_mutex_destroy(pthread_mutex_t *mutex);
```

### Semaphores

A counter-based synchronisation tool. Useful for limiting concurrent access to a resource.

```c
#include <semaphore.h>

sem_t sem;
sem_init(&sem, 0, 1);  // Initial value = 1 (binary semaphore)

sem_wait(&sem);   // Decrement (lock)
// critical section
sem_post(&sem);   // Increment (unlock)

sem_destroy(&sem);
```

### Condition Variable

Used to **block a thread until a specific condition becomes true**. Always used together with a mutex.

```c
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t  cond = PTHREAD_COND_INITIALIZER;

// Consumer thread — waits
pthread_mutex_lock(&lock);
while (!ready)
    pthread_cond_wait(&cond, &lock);  // Releases lock while waiting
pthread_mutex_unlock(&lock);

// Producer thread — signals
pthread_mutex_lock(&lock);
ready = 1;
pthread_cond_signal(&cond);           // Wake up one waiting thread
pthread_mutex_unlock(&lock);
```

---

`pthread_cond_init` initialises a condition variable dynamically. Use when heap-allocated or when custom attributes (e.g. clock type) are needed. For static/global variables, use `PTHREAD_COND_INITIALIZER` instead.

```c
int pthread_cond_init(pthread_cond_t *cond, const pthread_condattr_t *attr);
```

| Parameter | Description |
| :--- | :--- |
| `cond` | Pointer to the condition variable to initialise |
| `attr` | Attributes; `NULL` = defaults |

---

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
