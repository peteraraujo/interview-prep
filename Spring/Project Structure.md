## Spring Boot Project Structure (Interview Notes)

### Standard Directory Layout (Maven/Gradle)
```
my-spring-boot-app/
├── pom.xml (or build.gradle)
├── mvnw / gradlew (wrappers)
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/myapp/
│   │   │       ├── MyAppApplication.java
│   │   │       ├── controller/
│   │   │       ├── service/
│   │   │       ├── repository/
│   │   │       ├── model/ (or domain/)
│   │   │       ├── config/
│   │   │       └── exception/
│   │   └── resources/
│   │       ├── application.yml (preferred) or application.properties
│   │       ├── static/
│   │       └── templates/
│   └── test/
│       └── java/
│           └── com/example/myapp/
```

### Root Level Files
- **pom.xml / build.gradle**: Defines project metadata, Java version, Spring Boot Starters, and dependencies.
- **mvnw / gradlew**: Maven/Gradle wrappers — allow building without local installation of build tools.

### Main Application Class
- Located at the **root package** (`com.example.myapp`).
- Annotated with **`@SpringBootApplication`**.
- Contains the `main()` method.
- **Critical Rule**: Component scanning (`@ComponentScan`) starts from this class's package and all sub-packages. Placing components outside this hierarchy means they will be ignored.

### Layered Architecture (Most Common)

#### controller/
- Contains `@RestController` classes.
- Handles HTTP requests/responses (endpoints).
- Should remain **thin** — delegate business logic to services.

#### service/
- Contains `@Service` classes.
- Implements business rules and orchestration.
- Sits between controllers and repositories.

#### repository/
- Contains interfaces extending `JpaRepository` (Spring Data JPA).
- Handles all database operations (CRUD).
- Implementations are generated automatically at runtime.

#### model/ (or domain/)
- **Entities**: `@Entity` classes mapped to database tables.
- **DTOs** (Data Transfer Objects): Used for API communication to avoid exposing internal entity details (e.g., passwords).

#### config/
- `@Configuration` classes.
- Custom beans, security configuration (CORS, JWT), external client setups.

#### exception/
- Custom exceptions (e.g., `UserNotFoundException`).
- Global handler using `@ControllerAdvice` for standardized error responses.

### Resources Directory (`src/main/resources`)
- **application.yml** (or `.properties`): Main configuration file (ports, databases, logging, custom properties).
- **static/**: CSS, JavaScript, images (for REST APIs, often empty).
- **templates/**: Server-side templates (Thymeleaf, FreeMarker, etc.) for HTML rendering.

### Test Directory (`src/test/java`)
- Mirrors main package structure.
- Unit tests: Isolated service/repository tests.
- Integration tests: Use `@SpringBootTest` + `MockMvc` or `TestRestTemplate`.

### Alternative: Package-by-Feature (Domain-Driven Design)
Recommended for larger applications:
```
com/example/myapp/
├── order/
│   ├── OrderController.java
│   ├── OrderService.java
│   ├── OrderRepository.java
│   └── OrderEntity.java
├── user/
│   ├── UserController.java
│   ├── UserService.java
│   ├── UserRepository.java
│   └── UserEntity.java
```
**Benefits**: Easier feature isolation, better maintainability, and simpler future microservice extraction.

### Key Interview Points
- Spring Boot relies heavily on **package structure** for auto-configuration and component scanning.
- Prefer **Layered Architecture** for small/medium apps; shift to **Package-by-Feature** for large monoliths.
- Always place the `@SpringBootApplication` class at the highest package level.
- Use `application.yml` over `.properties` for better readability in modern projects.
- Keep controllers thin, services rich, and use DTOs to separate API contracts from domain models.

**Pro Tip**: When asked about structure, mention both layered and feature-based approaches, and explain how it impacts component scanning and maintainability.

