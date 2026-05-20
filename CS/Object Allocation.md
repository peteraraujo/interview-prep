## Object Allocation: Stack vs Heap

Object allocation is the process by which a running program requests and reserves a block of memory to store a new object (for example, a user profile, list, or database connection).

To understand allocation, you must distinguish between the two primary memory regions: the **Stack** and the **Heap**.

### 1. The Stack (Fast and Strict)
The stack is a highly organized, high-speed memory region that operates on a strict Last-In, First-Out (LIFO) basis.

- It stores local variables, function call frames, and primitive data types (numbers, booleans).
- The compiler must know the exact size of every item at compile time.
- Because object sizes can change dynamically, actual objects cannot live on the stack.

### 2. The Heap (Large and Dynamic)
The heap is a large, unstructured pool of memory used for dynamic allocation.

- It stores the actual data for complex objects.
- Finding and managing space here is slower than on the stack because the heap is unorganized.

### The Allocation Process
When code executes `User myUser = new User();`, a two-step process occurs:

- **Heap Allocation:** The program calculates the exact byte size needed for the `User` object, searches the heap for a contiguous free block, reserves it, and writes the object data there.
- **Stack Reference:** The heap returns a memory address (for example, `0x8FA4`). This address is stored as the variable `myUser` on the fast local stack.

The stack variable is therefore only a **pointer/reference** — it does not contain the object itself.

### Deallocation (Cleanup)
When the function that created `myUser` ends:

- The stack immediately pops its local variables, destroying the pointer `myUser`.
- The actual object remains in the heap but is now unreachable.

In garbage-collected languages (Java, JavaScript, C#), the Garbage Collector eventually reclaims the orphaned object. In languages like C++, you must explicitly delete it, or it becomes a memory leak.