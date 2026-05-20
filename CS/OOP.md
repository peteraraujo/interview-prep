## Object-Oriented Programming (OOP)

Object-Oriented Programming (OOP) is a software design paradigm that organizes code around data and the behavior that manipulates it, rather than around standalone functions and data structures. It combines state (data) and behavior (methods) into cohesive units called objects.

### Core Structure
- **Class:** A blueprint or template that defines the structure (properties and methods) an object will have. It contains no actual data.
- **Object (Instance):** A concrete realization of a class allocated in memory. Multiple distinct objects can be created from a single class.

### The Four Pillars of OOP
The four foundational principles of OOP are most clearly understood through the example of building a UI library (buttons, checkboxes, dropdowns, etc.).

1. **Encapsulation**  
   Encapsulation bundles data and the methods that operate on it into a single object and restricts direct external access to the internal state.  
   - **Why it matters:** It protects data integrity and makes the system predictable.  
   - **Example:** A `Button` object keeps its `isHovered` state private. External code cannot set `button.isHovered = true` directly. It must call a public method such as `button.triggerMouseEnter()`, ensuring the object maintains full control over its own state.

2. **Abstraction**  
   Abstraction hides complex internal implementation details and exposes only the essential, high-level interface needed to use the object.  
   - **Why it matters:** It reduces cognitive load for developers using the object.  
   - **Example:** A `VideoPlayer` object provides a simple public method `player.play()`. The caller does not need to know about memory buffers, H.264 decoding, or audio synchronization with the operating system.

3. **Inheritance**  
   Inheritance allows a new class to inherit properties and methods from an existing class, enabling code reuse and establishing hierarchical relationships.  
   - **Why it matters:** It eliminates duplicate code across similar classes.  
   - **Example:** A base `UIElement` class contains shared properties (`x`, `y`, `width`, `height`) and rendering logic. `SubmitButton`, `Checkbox`, and `Dropdown` inherit from `UIElement`, receiving all base functionality automatically while adding only their unique behavior.

4. **Polymorphism**  
   Polymorphism allows objects of different classes to be treated uniformly through a shared interface, even though their underlying implementations differ.  
   - **Why it matters:** It enables flexible, generic code that works with many object types without custom branching logic.  
   - **Example:** A rendering system maintains an array of UI elements (`Button`, `Checkbox`, `Dropdown`). It simply calls `.draw()` on each item in a loop. Thanks to polymorphism (all inherit from `UIElement`), the correct drawing behavior executes for each specific type without any `if/else` checks.

