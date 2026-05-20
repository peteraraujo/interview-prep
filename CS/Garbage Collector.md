## Garbage Collection (GC)

A Garbage Collector (GC) is a background process that automatically manages memory in languages such as Java, Python, JavaScript, and C#. It frees developers from manually releasing memory for objects that are no longer needed.

When an object is created, memory is allocated on the Heap. When the object is no longer required, the GC identifies it, deletes it, and returns the memory to the operating system.

### The Concept of Reachability
The GC determines whether an object is still needed by using **reachability** — it treats memory as a graph and starts from known entry points called **Roots**.

- **Roots** are always-accessible references: global variables and the local variables currently on the active Call Stack (i.e., functions that are still executing).
- **Tracing Process:** From each Root, the GC follows every pointer and reference. If Root A points to Object B, and Object B points to Object C, all three are considered reachable (alive).
- **Garbage:** Any object on the Heap that has no path back to any Root is unreachable and marked as garbage.

### The Most Common Algorithm: Mark-and-Sweep
Most modern GCs use a variation of the **Mark-and-Sweep** algorithm, which runs in two phases:

- **Mark Phase:** The GC temporarily pauses the program (“stop-the-world” pause). It starts at the Roots and traverses every reference path. Each reachable object has a tiny boolean flag flipped to “marked.”
- **Sweep Phase:** The GC scans the entire Heap. Marked objects have their flag reset and are left alone. Unmarked objects are immediately deleted and their memory is freed.

### Why Not Use Garbage Collection Everywhere?
Even though GC is convenient, languages like C, C++, and Rust still require manual memory management for two main reasons:

- **Performance Overhead:** The GC constantly consumes CPU cycles to scan and trace memory.
- **Unpredictability (Pauses):** The Mark phase must pause the main thread to safely trace a moving target. In latency-sensitive applications (e.g., video games at 60 fps), even a 20 ms pause causes visible stuttering or dropped frames.

