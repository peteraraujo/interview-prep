## Mutexes vs Semaphores

Mutexes and semaphores are synchronization primitives that prevent race conditions when multiple threads access shared resources. While both solve the same core problem, they are designed for different scenarios and follow distinct rules.

### 1. Mutex (Mutual Exclusion)
A mutex is a locking mechanism that guarantees **exactly one thread** can access a protected resource at any time.

**Real-World Analogy:**  
A coffee shop with a single bathroom and one physical key at the counter.  
- Thread A requests the key, locks the door, and uses the bathroom.  
- Thread B must wait until Thread A returns the key.

**Crucial Rule:** Strict ownership. Only the thread that acquired (locked) the mutex is allowed to release (unlock) it.

**When to Use:**  
When modifying shared data (e.g., updating a bank balance, writing to a file, or incrementing a counter) where absolute exclusivity is required.

### 2. Semaphore
A semaphore is a signaling mechanism built around a counter. It controls access to a resource that can be used by multiple threads simultaneously, up to a defined limit.

**Real-World Analogy:**  
A parking garage with 3 available spaces and an electronic sign showing the current count.  
- The semaphore starts with a count of 3.  
- Each thread that enters decrements the counter.  
- When the count reaches 0, new threads must wait.  
- When a thread leaves, the counter increments and the next waiting thread is allowed in.

**Crucial Rule:** No ownership. Any thread can increment (signal) or decrement the counter. A different thread can release the resource.

**Note:** A semaphore initialized with a maximum count of 1 is called a **binary semaphore**. It behaves similarly to a mutex but lacks the strict ownership rule.

### Summary of Differences

| Feature              | Mutex                                      | Semaphore                                      |
|----------------------|--------------------------------------------|------------------------------------------------|
| Primary Purpose      | Mutual exclusion (exclusive locking)       | Signaling and resource counting                |
| Capacity             | Exactly 1 thread at a time                 | N threads at a time (based on counter value)   |
| Ownership            | Strict: only the locking thread can unlock | None: any thread can signal/increment          |
| Behavior Type        | Locking mechanism                          | Signaling mechanism                            |

Mutexes are ideal for protecting critical sections of code that must run atomically. Semaphores excel at managing pools of limited resources (e.g., database connections, worker threads, or parking spaces).

