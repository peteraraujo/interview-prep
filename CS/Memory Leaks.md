## Memory Leaks

A memory leak occurs when a program allocates memory for a task but fails to release that memory back to the operating system when the task completes. The operating system continues to treat the memory as in use, even though the program can no longer access it. Over time, leaked memory accumulates as “dead weight,” increasing overall RAM consumption and eventually causing system slowdowns or an Out of Memory (OOM) crash.

### The Lifecycle of Memory
Every piece of data in an application follows a standard lifecycle:

- **Allocation:** The program requests a block of memory from the operating system (for example, creating a user object or loading an image).
- **Usage:** The program reads from and writes to that memory.
- **Release (Deallocation):** The program explicitly notifies the operating system that the memory is no longer needed.

A memory leak occurs when the release step is skipped or becomes impossible to perform.

### How Memory Leaks Occur (By Language Type)

#### 1. Manual Memory Management (C, C++, Rust)
In lower-level languages, developers are fully responsible for the entire memory lifecycle. Memory is allocated with `malloc()` in C or `new` in C++ and must be explicitly freed with `free()` or `delete`.

A leak happens when the developer forgets the release call or an error path skips it. Once the pointer (the variable holding the memory address) goes out of scope, the memory becomes permanently orphaned and unreachable.

#### 2. Garbage-Collected Languages (Java, Python, JavaScript, C#)
These languages use an automatic background process called the Garbage Collector (GC). The GC scans for objects that have no remaining references (“unreachable”) and frees their memory automatically.

Leaks still occur when a reference to an unneeded object is accidentally retained.  
**Common Example:** A temporary user object is added to a global list or cache. When the user logs out, the object is not removed from the list. The Garbage Collector sees the global reference and never reclaims the memory.

### The Consequences
Memory leaks are difficult to detect because they do not cause immediate crashes. They act as “silent killers,” gradually consuming more RAM (sometimes only 1 MB per minute).  

Eventually the system exhausts physical RAM and begins using swap space or page files on disk, which dramatically slows performance. When no memory remains, the operating system forcibly terminates the program to protect the rest of the system.

