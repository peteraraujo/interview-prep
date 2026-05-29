## Virtual Threads (Project Loom)

### The Problem with Traditional Platform Threads
Platform threads (the classic Java `Thread`) map 1:1 with operating system threads. They are expensive:

- Each thread consumes approximately 1MB of stack memory.
- Context switching requires kernel intervention and is relatively slow.
- In the traditional "thread-per-request" model, a blocking operation (database call, network I/O, `sleep()`) keeps the entire OS thread idle, even though it is doing no work.

This leads to thread exhaustion and high memory usage long before CPU limits are reached.

### The Failed Workaround: Asynchronous / Reactive Programming
Reactive approaches (Spring WebFlux, CompletableFuture, callbacks) avoid blocking by freeing the thread during I/O waits. While efficient, they introduce significant complexity:

- Difficult to read and maintain code.
- Broken stack traces.
- Loss of natural control flow (try/catch, loops).

### The Solution: Virtual Threads (Java 21+)
Virtual Threads introduce an M:N scheduling model. Millions of lightweight Virtual Threads run on top of a small number of Platform (Carrier) Threads.

**Key Mechanics**:
- **Carrier Threads**: Small pool of real OS threads (typically one per CPU core).
- **Mounting**: A Virtual Thread is mounted onto a Carrier Thread when it needs to execute CPU work.
- **Unmounting**: When the Virtual Thread blocks (I/O, sleep, database call), the JVM unmounts it, stores its stack in heap memory, and frees the Carrier Thread for other work.
- **Resumption**: When the blocking operation completes, the Virtual Thread is remounted onto any available Carrier Thread and continues execution.

Virtual Threads are cheap objects stored in the heap. They consume only a few hundred bytes each instead of megabytes, allowing millions to exist simultaneously.

### Major Behavioral Changes (New Rules)

**No Thread Pooling**  
Virtual Threads are extremely cheap to create and destroy. Traditional fixed-size thread pools are no longer necessary. Use:
```java
Executors.newVirtualThreadPerTaskExecutor()
```
Create a new Virtual Thread per task and let it die after completion.

**Pinning**  
A Virtual Thread becomes pinned to its Carrier Thread when it blocks inside:
- `synchronized` blocks/methods, or
- Native code (JNI).

Pinned threads defeat the benefits of Virtual Threads. Modern code should prefer `ReentrantLock` over `synchronized`.

**ThreadLocal Limitations**  
`ThreadLocal` variables become dangerous with millions of Virtual Threads, easily causing `OutOfMemoryError`.  
Java 21 introduced **Scoped Values** as a lightweight, memory-safe replacement for per-thread data.

### Summary of Benefits
- Write simple, blocking, synchronous code.
- Achieve the scalability of asynchronous models.
- Dramatically higher concurrency with lower memory usage.
- Much easier debugging and maintainability compared to reactive code.

Virtual Threads represent the biggest shift in Java concurrency since the introduction of the `java.util.concurrent` package.

