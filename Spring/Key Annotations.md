## Spring Boot Annotations

### 1. Core Stereotypes (Component Scanning)
These annotations mark classes so Spring's IoC container can discover them via component scanning, instantiate them as beans (singletons by default), and manage their lifecycle.

- **`@Component`**  
  Foundational generic annotation. Used for any utility class, custom formatter, or bean that does not fit a specific architectural layer.

- **`@Service`**  
  Applied to the business logic layer. Signals that the class contains core application rules and orchestration logic. Spring treats it the same as `@Component` but provides semantic clarity.

- **`@Repository`**  
  Used on data access classes or interfaces. Special behavior: Spring automatically translates low-level persistence exceptions (e.g., `SQLException`) into Spring's consistent `DataAccessException` hierarchy.

- **`@Controller`**  
  Used in traditional Spring MVC applications that return HTML views (e.g., Thymeleaf templates). Not typically used in pure REST APIs.

**Important Note**: All of these ultimately register beans. Using the correct one communicates intent and enables layer-specific features.

### 2. Dependency Injection and Wiring

- **`@Autowired`**  
  Instructs Spring to inject a matching bean from the IoC container.  
  Modern best practice: Place on the **constructor**. If a class has only one constructor, Spring Boot performs implicit injection even without the annotation. Field injection is still supported but discouraged.

- **`@Qualifier("beanName")`**  
  Used with `@Autowired` to resolve ambiguity when multiple beans of the same type exist.  
  Example: Two implementations of `ReportFormatter` (`PdfFormatter` and `ExcelFormatter`). `@Qualifier` explicitly selects one.

### 3. Configuration and Manual Bean Creation
Used when you cannot annotate a class directly (e.g., third-party libraries).

- **`@Configuration`**  
  Marks a class as a source of bean definitions. Spring processes these classes early during startup.

- **`@Bean`**  
  Used inside `@Configuration` classes on methods. The return value of the method becomes a managed bean in the Spring container.  
  Common for configuring third-party components (e.g., `PasswordEncoder`, `RestTemplate`, AWS clients).

### 4. RESTful Web Services

- **`@RestController`**  
  Convenience annotation equivalent to `@Controller + @ResponseBody`. Every method returns data (JSON, XML, etc.) directly to the client instead of rendering a view.

**Request Mapping Annotations**:
- **`@GetMapping`**, **`@PostMapping`**, **`@PutMapping`**, **`@PatchMapping`**, **`@DeleteMapping`**, **`@RequestMapping`**  
  Define the HTTP method and URL path for a handler method.

**Data Extraction Annotations**:
- **`@PathVariable`** — Binds URI template variables (e.g., `/users/{id}`).
- **`@RequestParam`** — Binds query parameters (e.g., `/users?role=admin`).
- **`@RequestBody`** — Deserializes the entire request body (usually JSON) into a Java object using Jackson.

### 5. Environment and Property Binding

- **`@Value("${property.name}")`**  
  Injects a single property value from `application.yml`, `application.properties`, environment variables, or command-line arguments.  
  Simple but can scatter configuration logic across the codebase.

- **`@ConfigurationProperties(prefix = "app.mail")`**  
  Preferred enterprise approach. Binds a group of related properties into a strongly-typed object or record. Supports validation, nested properties, and IDE autocompletion.  
  Example: `app.mail.host`, `app.mail.port`, `app.mail.password` all map into one `MailProperties` class.

### 6. Transaction Management

- **`@Transactional`**  
  One of the most important annotations in Spring.  
  When placed on a service method (or class), Spring creates a proxy that wraps the method in a database transaction.  
  - Success → automatic commit.  
  - Any unchecked exception (`RuntimeException` or subclass) → automatic rollback.  
  Can be configured with propagation behavior, isolation level, timeout, and `readOnly=true`.

**Recommended Placement**: Service layer (not controllers or repositories).

