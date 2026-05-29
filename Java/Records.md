## Java Records

### Architectural Definition
A Record is a special kind of class designed specifically for immutable data carriers. It is declared with its state directly in the class header. The Java compiler automatically generates the boilerplate code required for data classes.

When a Record is declared:
- The class is implicitly `final` (cannot be subclassed).
- It implicitly extends `java.lang.Record`.
- Each component in the header becomes a `private final` field.

### Compiler-Generated Members
The compiler automatically provides:
- A **canonical constructor** that accepts all components and assigns them to the private final fields.
- Public accessor methods with the same name as the component (e.g., `id()`, `username()` — not `getId()`).
- `equals()` implementation based on all components.
- `hashCode()` implementation based on all components.
- `toString()` that includes the record name and all component values.

### Basic Syntax and Usage
```java
public record User(int id, String username, String email) {}
```

```java
User user = new User(1, "admin", "admin@system.local");

int id = user.id();                    // Accessor
String username = user.username();

System.out.println(user);              // Auto-generated toString()
```

### Compact Constructor (Validation & Normalization)
Records support a special compact constructor for validation logic without repeating parameters:

```java
public record User(int id, String username, String email) {

    public User {  // Compact constructor - no parameter list
        if (id <= 0) {
            throw new IllegalArgumentException("ID must be positive");
        }
        if (username == null || username.isBlank()) {
            throw new IllegalArgumentException("Username cannot be blank");
        }
        // Normalization (compiler auto-assigns fields at the end)
        this.email = (email != null) ? email.toLowerCase() : null;
    }
}
```

### Customization Options
- Can implement interfaces.
- Can add static fields and static methods.
- Can override generated accessor methods, `equals()`, `hashCode()`, or `toString()`.
- Can add instance methods (but not additional instance fields).

### Strict Restrictions
- Cannot extend another class.
- Cannot declare additional non-static instance fields.
- Cannot be `abstract`.
- All state must be declared in the record header (immutable by design).
- No setter methods allowed.

### Serialization Behavior
Records are serialized more securely than regular classes. During deserialization, the **canonical constructor** is always invoked. This guarantees that validation logic defined in the compact constructor is executed, preventing creation of invalid record states from malicious or corrupted data.

