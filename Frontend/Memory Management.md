### Memory Allocation

Browsers divide memory into two primary regions during JavaScript execution:

* **The Stack:** A structured, fixed-size region used for static data. It stores primitive values (numbers, strings, booleans) and memory addresses (pointers) of objects. Allocation is managed sequentially as functions are called and return.
* **The Heap:** An unstructured region used for dynamic data. It stores objects, arrays, and functions. Memory is allocated here when the exact size of the data cannot be determined prior to execution.

### Garbage Collection (GC)

Browsers utilize automated garbage collection to reclaim memory within the heap. The mechanism relies strictly on the concept of "reachability."

* **Roots:** The garbage collector establishes a baseline set of roots. In a browser, the primary root is the global object (`window`). Local variables within the currently executing function are also considered temporary roots.
* **Mark Phase:** The GC algorithm traverses the object graph starting from the roots. It follows every variable reference. Any object successfully reached during this traversal is explicitly marked as active.
* **Sweep Phase:** The GC scans the heap memory. Any object that does not possess an active mark is classified as unreachable. The engine deletes these objects and frees the underlying memory blocks.

### Standard Memory Leaks

A memory leak occurs when an application retains references to data that is no longer required, preventing the garbage collector from executing the sweep phase on that data.

* **Global Variables:** Variables declared without scoping keywords (`var`, `let`, `const`) or intentionally assigned to the `window` object remain permanently reachable and are never collected.
* **Detached DOM Nodes:** If a developer removes an HTML element from the active Document Object Model (DOM) but retains a JavaScript variable pointing to that element, the element and all of its child nodes are preserved in memory.
* **Uncleared Event Listeners:** An event listener attached to a DOM element retains a reference to its callback function. If the element is removed from the DOM but the listener is not explicitly removed via `removeEventListener`, the function and its lexical scope remain active in memory.
* **Improper Closures:** An inner function maintains a reference to variables within its parent scope. If the inner function is kept alive (e.g., assigned to a global variable or a long-lived timer), the variables in the parent scope cannot be collected, regardless of whether they are actively used.