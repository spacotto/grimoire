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

### 1. Creating & Joining Threads

```c
#include <pthread.h>
#include <stdio.h>

void *worker(void *arg)
{
    printf("Thread running\n");
    return NULL;
}

int main()
{
    pthread_t tid;
    pthread_create(&tid, NULL, worker, NULL);  // Create
    pthread_join(tid, NULL);                   // Wait for completion
    return 0;
}
```

| Function | Purpose |
|---|---|
| `pthread_create()` | Spawns a new thread |
| `pthread_join()` | Waits for a thread to finish |
| `pthread_exit()` | Terminates calling thread |
| `pthread_self()` | Returns the calling thread's ID |

### 2. Mutex (Mutual Exclusion)

Prevents two threads from accessing a critical section simultaneously.

```c
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;

void *safe_increment(void *arg)
{
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

### 3. Semaphores

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

| Value | Meaning |
|---|---|
| `= 1` | Binary semaphore (like a mutex) |
| `> 1` | Counting semaphore (N simultaneous accesses) |

### 4. Condition Variables

Used to **block a thread** until a specific condition becomes true.

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

>[!IMPORTANT]
>Always use a `while` loop (not `if`) to recheck the condition after waking—**spurious wakeups** can occur.

### 5. Read-Write Locks

Allows **multiple readers** or **one writer** at a time. Ideal for read-heavy workloads.

```c
pthread_rwlock_t rwlock = PTHREAD_RWLOCK_INITIALIZER;

pthread_rwlock_rdlock(&rwlock);   // Shared read lock
// read data
pthread_rwlock_unlock(&rwlock);

pthread_rwlock_wrlock(&rwlock);   // Exclusive write lock
// modify data
pthread_rwlock_unlock(&rwlock);
```

## Concurrency Issues

### Race Condition
Two threads read/write shared data simultaneously → unpredictable result.

```c
// BAD: counter++ is not atomic (read → modify → write)
counter++;

// FIX: wrap in mutex lock/unlock
```

### 2. Deadlock
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

### 3. Starvation
A thread is perpetually denied access to a resource because others keep taking priority.

**Fix:** Use fair scheduling policies or bounded waiting mechanisms.

### 4. Priority Inversion
A high-priority thread is blocked by a low-priority thread holding a needed lock.

**Fix:** Use **priority inheritance** (OS-level) or redesign lock usage.

### 5. False Sharing
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
