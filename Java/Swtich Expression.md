## Java Switch Expressions & Pattern Matching

### 1. Switch Expressions (Java 14+)
Switch can now be used as an **expression** that returns a value, rather than only as a statement.

**Key Features**:
- Arrow syntax (`->`) replaces colon (`:`) and prevents fall-through (no `break` needed).
- Multiple case labels supported with commas.
- `yield` keyword returns a value from a multi-line code block.
- **Exhaustiveness**: Compiler enforces that all possible values are covered (or a `default` is present).

**Example**:
```java
public int getDaysInMonth(Month month) {
    return switch (month) {
        case JANUARY, MARCH, MAY, JULY, AUGUST, OCTOBER, DECEMBER -> 31;
        case APRIL, JUNE, SEPTEMBER, NOVEMBER -> 30;
        case FEBRUARY -> {
            boolean isLeapYear = checkLeapYear();
            yield isLeapYear ? 29 : 28;
        }
    };  // Semicolon required for expressions
}
```

### 2. Pattern Matching for Switch (Java 21+)
Switch selector can now match on **types**, **patterns**, and **conditions**, not just constant values.

**Key Features**:
- **Type Patterns**: Match by type with automatic casting and variable binding (`case String s`).
- **Guard Clauses**: `when` keyword adds boolean conditions to a case.
- **Null Handling**: Explicit `case null` support (no more automatic `NullPointerException`).
- **Record Patterns**: Deconstruct records directly in the case label, extracting components into variables.

**Example**:
```java
public String processObject(Object obj) {
    return switch (obj) {
        case null -> "Object is explicitly null";
        
        case String s when s.isEmpty() -> "Empty string provided";
        case String s -> "String of length: " + s.length();
        
        case Integer i -> "Integer multiplied: " + (i * 2);
        
        // Record deconstruction
        case Point(int x, int y) -> "Record extracted. X: " + x + ", Y: " + y;
        
        default -> "Unknown object type: " + obj.getClass().getSimpleName();
    };
}
```

### Major Benefits
- Eliminates verbose `if-else` chains and traditional switch fall-through bugs.
- Enables expressive, type-safe matching with compile-time exhaustiveness checks.
- Improves readability and reduces boilerplate for complex conditional logic.
- Works seamlessly with sealed classes and records for pattern matching hierarchies.

