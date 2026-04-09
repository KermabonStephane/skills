---
name: Spring Boot 4 Skills
description: Guide for developing modern, high-performance Spring Boot 4 applications with Java 25, Spring Modulith, and GraalVM.
---

# Modern Spring Boot 4 Development Guide

This skill provides guidelines and best practices for building robust, scalable, and high-performance applications using **Spring Boot 4**, focusing on **Java 25 LTS**, cloud-native patterns, and developer productivity.

## 🏛️ Architecture: Spring Modulith
Avoid the complexity of microservices by starting with a modular monolith.

1.  **Logical Modules**: Organize code into domain-driven packages.
2.  **Encapsulation**: Keep internal implementation details package-private. Only expose necessary APIs.
3.  **Event-Driven**: Use Spring's Application Events for decoupled communication between modules.
4.  **Verification**: Use `Spring Modulith` testing support to ensure no illegal dependencies between modules.

## 🚀 Key Spring Boot 4 & Java 25 Features

### 1. Virtual Threads (Project Loom)
-   **High Throughput**: Enabled by default for Tomcat and Jetty. Ensure `spring.threads.virtual.enabled=true` is set.
-   **Blocking I/O**: No longer a bottleneck. Prefer standard WebMVC with Virtual Threads over complex WebFlux where possible.

### 2. Native Compilation (GraalVM)
-   **AOT (Ahead-of-Time)**: Optimize applications for GraalVM Native Image to achieve near-instant startup and reduced memory usage.
-   **Cloud Native**: Ideal for serverless and containerized environments (Kubernetes).

### 3. Checkpoint/Restore (CRaC)
-   Support for **Project CRaC** to save the JVM state and restore it instantly, bypassing heavy startup tasks.

## 🌐 Web & API Development

-   **RestClient**: Use the functional `RestClient` for synchronous HTTP calls with a modern API.
-   **HTTP Interfaces**: Define declarative clients using `@HttpExchange` interfaces.
-   **Problem Details**: Standardized error responses using `ProblemDetail` (RFC 7807).

## 💾 Data & Persistence

-   **Spring Data JPA**: Leverages **Jakarta Persistence 3.2+** and **Hibernate 7+**.
-   **Records in JPA**: Use Java Records for projections and DTOs.
-   **Migrations**: Always use **Flyway** or **Liquibase** for database schema versioning.

## 🛡️ Security & Observability

-   **Spring Security**: Implementation of OAuth2, OIDC, and SAML. Use the latest functional DSL for configuration.
-   **Observability**: Integrated **Micrometer** and **OpenTelemetry** support. Metrics, traces, and logs are unified under the OTLP standard.

## 🧪 Testing with Spock & Testcontainers
Always use **Spock Framework** for testing Spring Boot applications.

-   **@SpringBootTest**: For full context integration tests.
-   **Testcontainers**: Use `@ServiceConnection` to automatically configure databases and brokers (Kafka, Redis) for tests.
-   **Spock Specs**: Use `given:`, `when:`, `then:` blocks to test Spring beans and controllers.

## ✨ Best Practices
-   **Constructor Injection**: Always use constructor-based DI. Never use `@Autowired` on fields.
-   **Type-safe Config**: Use `@ConfigurationProperties` with Records for application settings.
-   **Immutability**: Use Java Records for all DTOs, API models, and Event payloads.
-   **Lean Controllers**: Keep business logic in Services or Use Cases; Controllers only handle request/response mapping.

## 🛠️ Project Templates

- **Maven Standalone POM**: Modern `pom.xml` for single-module projects.
  `./templates/pom.xml`
- **Maven Parent POM**: Multi-module parent configuration.
  `./templates/pom-parent.xml`
- **Maven Module POM**: Individual module configuration inheriting from parent.
  `./templates/pom-module.xml`


## 📂 Standard Project Structure (Modulithic)
```
src/main/java/com/example/app/
  shared/             # Common utilities, base classes
  order/              # Order Domain (Module)
    Order.java        # Entity
    OrderService.java # Business Logic
    OrderRepository.java
    web/              # Order Controllers & DTOs
  inventory/          # Inventory Domain (Module)
  shipping/           # Shipping Domain (Module)
```

---
Using this skill ensures Spring Boot 4 applications are built with the highest standards of modern Java development, performance, and maintainability.
