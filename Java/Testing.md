## Software Testing & Unit Testing

### The Anatomy of a Unit Test
A well-structured unit test follows the **Arrange-Act-Assert (AAA)** pattern (also known as Given-When-Then):

- **Arrange (Given)**: Set up the test fixture, create objects, and prepare input data.
- **Act (When)**: Execute the method or function under test.
- **Assert (Then)**: Verify that the actual output matches the expected result.

### Code Example: Calculator Class
```java
public class Calculator {

    public int add(int a, int b) {
        return a + b;
    }

    public double divide(int numerator, int denominator) {
        if (denominator == 0) {
            throw new IllegalArgumentException("Cannot divide by zero.");
        }
        return (double) numerator / denominator;
    }
}
```

### Corresponding JUnit 5 Test Class
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class CalculatorTest {

    @Test
    public void testAdd_PositiveNumbers_ReturnsCorrectSum() {
        // Arrange
        Calculator calculator = new Calculator();
        int expectedResult = 15;

        // Act
        int actualResult = calculator.add(10, 5);

        // Assert
        assertEquals(expectedResult, actualResult, "10 + 5 should equal 15");
    }

    @Test
    public void testDivide_ValidInput_ReturnsCorrectQuotient() {
        // Arrange
        Calculator calculator = new Calculator();
        double expectedResult = 2.5;

        // Act
        double actualResult = calculator.divide(5, 2);

        // Assert
        assertEquals(expectedResult, actualResult, 0.001);  // Delta for floating point
    }

    @Test
    public void testDivide_ByZero_ThrowsException() {
        // Arrange
        Calculator calculator = new Calculator();

        // Act & Assert
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            calculator.divide(10, 0);
        });

        assertEquals("Cannot divide by zero.", exception.getMessage());
    }
}
```

### Key Testing Terminology
- **Assertion**: A statement that must be true for the test to pass (e.g., `assertEquals`, `assertThrows`, `assertTrue`).
- **Code Coverage**: Percentage of production code lines or branches executed by the test suite.
- **Test-Driven Development (TDD)**: Write failing tests first, then implement the minimal code to make them pass.
- **Mocking**: Replacing real dependencies (databases, external services, complex objects) with fake implementations to test a unit in complete isolation.

### Unit Testing Principles
- Tests focus on individual methods or classes in isolation.
- Each test should be fast, independent, and deterministic.
- Good unit tests validate both happy paths and edge cases (nulls, zero, exceptions).
- Unit tests form the foundation of the testing pyramid (Unit → Integration → System/UI).

This structure ensures tests are readable, maintainable, and provide clear failure messages when something breaks.

