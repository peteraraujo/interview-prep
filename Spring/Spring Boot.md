## Spring Boot

### Spring vs Spring Boot
**Traditional Spring**:
- Unopinionated, powerful but requires heavy manual configuration (XML or Java config).
- Manual dependency management and wiring.

**Spring Boot**:
- Opinionated framework built on Spring.
- Philosophy: **Convention over Configuration**.
- Provides production-ready defaults, auto-configuration, and embedded servers.
- Goal: Focus on business logic instead of infrastructure.

| Feature              | Traditional Spring                  | Spring Boot                          |
|----------------------|-------------------------------------|--------------------------------------|
| Philosophy           | Unopinionated                       | Opinionated (sensible defaults)      |
| Configuration        | Heavy boilerplate                   | Auto-configuration                   |
| Dependencies         | Manual version management           | Starter dependencies                 |
| Deployment           | WAR + external server               | Executable JAR + embedded server     |
| Production Readiness | Custom built                        | Built-in Actuator                    |

### The Four Pillars of Spring Boot

#### 1. Auto-Configuration
- Core magic of Spring Boot.
- Inspects classpath and automatically configures beans (DataSource, EntityManager, etc.).
- Example: Detecting H2 → auto-configures in-memory database.
- Can be overridden with custom configuration.

#### 2. Starter Dependencies
- Curated bundles of compatible libraries.
- Common Starters:
  - `spring-boot-starter-web`: Tomcat + Spring MVC + Jackson.
  - `spring-boot-starter-data-jpa`: Hibernate + Spring Data JPA.
  - `spring-boot-starter-security`: Authentication & authorization.
  - `spring-boot-starter-test`: Testing utilities.

#### 3. Embedded Servers
- No need for external Tomcat/Jetty.
- Packages server inside executable JAR.
- Run with `java -jar app.jar`.
- Simplifies Docker/container deployment.

#### 4. Actuator (Production Readiness)
- Exposes operational endpoints:
  - `/actuator/health` — Application & dependency health.
  - `/actuator/metrics` — JVM, HTTP, GC metrics.
  - `/actuator/env` — Environment & properties.
- Integrates with Micrometer for observability.

### Anatomy of a Spring Boot Application
- Main class annotated with **`@SpringBootApplication`**.
- This meta-annotation combines:
  - `@Configuration` — Bean definitions.
  - `@EnableAutoConfiguration` — Enables auto-config.
  - `@ComponentScan` — Scans for components, controllers, services.

### Externalized Configuration & Profiles
- `application.properties` or `application.yml` — Central config file.
- **Profiles**: Environment-specific configs (`application-dev.yml`, `application-prod.yml`).
- Activate via `--spring.profiles.active=prod`.
- Allows same codebase across dev/test/prod.

### Modern Spring Boot (2026+)
- **Java 17+** baseline (records, pattern matching, sealed classes).
- **GraalVM Native Image** support — AOT compilation for:
  - Millisecond startup.
  - Low memory footprint.
  - Ideal for serverless & Kubernetes.
- Strong **Observability** via Micrometer + OpenTelemetry + Prometheus.

### Key Interview Takeaways
- Spring Boot = Spring + sensible defaults + production tools.
- Understand **Auto-configuration** order and how to override it.
- Know common starters and when to use them.
- Be ready to discuss JAR vs WAR, embedded servers, and Actuator.
- Mention GraalVM native images for cloud-native discussions.
