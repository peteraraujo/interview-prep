## Java Core Concepts

### 1. Execution Environment
- **JVM (Java Virtual Machine)**: Abstract machine that executes Java bytecode.
- **JRE (Java Runtime Environment)**: Contains JVM + core class libraries required to run Java applications.
- **JDK (Java Development Kit)**: Full development kit including JRE, `javac` compiler, `jar` archiver, and tools.

**Platform Independence**: Source code compiles to platform-independent bytecode (`.class` files). The JVM (platform-specific) interprets or JIT-compiles the bytecode.

**Just-In-Time (JIT) Compiler**: Dynamically compiles hot bytecode paths into native machine code at runtime for performance.

### 2. Object-Oriented Principles
- **Encapsulation**: Bundles data and methods into a class. Controls access using `private`, `protected`, and `public` modifiers.
- **Inheritance**: Subclasses inherit fields and methods from superclasses. Java supports single class inheritance and multiple interface inheritance.
- **Polymorphism**:
  - **Compile-time (Static)**: Method overloading (same name, different parameters).
  - **Run-time (Dynamic)**: Method overriding (subclass provides specific implementation of superclass method).
- **Abstraction**: Hides implementation details. Achieved via abstract classes and interfaces.

### 3. Core Language Mechanics
- **Pass-by-Value**: Java always passes copies. Primitives pass their value; objects pass their reference (memory address).
- **`==` Operator**: Compares object references (memory equality).
- **`.equals()` Method**: Compares object content (logical equality). Must be overridden in custom classes (default behavior from `Object` is reference equality).
- **`static` Keyword**: Binds members to the class rather than instances. Shared across all objects.
- **`final` Keyword**:
  - Variables: Prevents reassignment.
  - Methods: Prevents overriding.
  - Classes: Prevents subclassing.
- **String Pool**: Special heap area for string literals. Identical literals reuse the same object to save memory.

### 4. Memory Management & Garbage Collection
- **Heap Memory**: Shared area for all object instances and arrays.
- **Stack Memory**: Per-thread area for local variables and method frames.
- **Garbage Collection (GC)**: Automatic reclamation of unreachable objects.
  - Starts from GC Roots (thread stacks, static fields, etc.).
  - Marks reachable objects; unreachable objects are swept.

### 5. Collections Framework (`java.util`)
| Collection       | Backing Structure          | Key Operations Time Complexity | Ordering          |
|------------------|----------------------------|--------------------------------|-------------------|
| ArrayList        | Dynamic array              | Access O(1), Insert/Delete O(n) | Insertion order   |
| LinkedList       | Doubly-linked list         | Insert/Delete O(1), Access O(n) | Insertion order   |
| HashSet          | HashMap                    | Add/Contains/Remove O(1) avg   | No order          |
| TreeSet          | Red-Black Tree             | Add/Contains/Remove O(log n)   | Sorted            |
| HashMap          | Array + LinkedList/Tree    | Get/Put O(1) avg               | No order          |
| TreeMap          | Red-Black Tree             | Get/Put O(log n)               | Sorted by key     |

### 6. Exception Handling
- **Checked Exceptions**: Extend `Exception` (not `RuntimeException`). Must be handled or declared (`throws`). Example: `IOException`.
- **Unchecked Exceptions**: Extend `RuntimeException`. Indicate programming errors. Example: `NullPointerException`.
- **try-catch-finally**: `finally` block always executes (used for resource cleanup).

### 7. Multithreading & Concurrency
- **Thread Creation**: Extend `Thread` or implement `Runnable`/`Callable`.
- **Synchronization**: `synchronized` keyword ensures mutual exclusion via intrinsic locks.
- **`volatile` Keyword**: Guarantees visibility of changes across threads by bypassing CPU caches.
- **Executors Framework**: High-level API (`ExecutorService`) for managing thread pools. Separates task submission from execution mechanics.

