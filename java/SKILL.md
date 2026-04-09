---
name: Java 25 Skills
description: Guide for developing modern, high-performance Java 25 applications with Clean Architecture and Spock testing.
---

# Modern Java 25 Development Guide

This skill provides guidelines and best practices for building robust, scalable, and high-performance applications using **Java 25 LTS** (released Sept 2025), focusing on Clean Architecture and expressive testing.

## 🏛️ Clean Architecture Implementation
Always follow the Dependency Rule: Source code dependencies only point inwards.

1.  **Entities (Core)**: Business objects, POJOs, or **Records**. No dependencies on frameworks.
2.  **Use Cases (Application)**: Business rules orchestrating flow between entities.
3.  **Interface Adapters**: Controllers, Gateways, and Mappers (**MapStruct**). Transforms data between external formats and internal entities.
4.  **Frameworks & Drivers**: Database (JPA/Hibernate), Web Framework (Spring/Jakarta), and External Services.

## 🚀 Key Java 25 Features

### 1. Project Loom & Concurrency
-   **Virtual Threads**: Use for high-throughput I/O. Prefer `Executors.newVirtualThreadPerTaskExecutor()`.
-   **Scoped Values (Final in 25)**: Use as a safer, memory-efficient alternative to `ThreadLocal` when sharing data across virtual threads.
-   **Structured Concurrency**: Treat groups of related tasks as a single unit using `StructuredTaskScope`.

### 2. Modern Language Syntax
-   **Flexible Constructor Bodies**: You can now execute logic (validation, logging) *before* calling `super()` or `this()`.
-   **Pattern Matching**: Extensive use in `switch` and `instanceof` (including primitive types in 25).
-   **String Templates**: Use for cleaner string interpolation (e.g., `STR."Hello \{name}"`).
-   **Records**: Default for immutable data carriers (DTOs, Events). Use for most "Entities" that don't require identity-based mutability.

### 3. Memory Optimization
-   **Compact Object Headers**: Leverages the 64-bit to 64-bit optimization to reduce memory footprint.
-   **Project Valhalla (Value Classes)**: Use Value Classes (if available/preview) for high-performance, identity-less data types to reduce GC pressure.

## 🛠️ Tooling & Boilerplate Reduction

-   **Lombok**: Use `@Builder`, `@Slf4j`, and `@SneakyThrows`. Apply to legacy classes or complex entities where Records aren't sufficient.
-   **MapStruct**: Generate type-safe mappers at compile-time. Use for transforming between Database Entities and API Records.
-   **Maven**: Always use the latest stable version of Maven for builds.

## 🧪 Next-Gen Testing with Spock
Always use the **Spock Framework** for all tests.

-   **Specification**: Use Groovy-based BDD specs (`given:`, `when:`, `then:`).
-   **Data-Driven**: Use `where:` blocks with Data Tables for exhaustive edge-case testing.
-   **Mocking**: Use Spock's built-in mocking (`Mock()`, `Stub()`) instead of Mockito.
-   **Java 25 Testing**: Test Virtual Thread behavior and Pattern Matching logic using Spock's expressive syntax.

## ✨ Clean Code Guidelines
-   **Functions**: Must be **small** (hardly ever > 20 lines) and **do one thing**.
-   **Naming**: Revealing intent. Use `CustomerRepository` (Interface) and `JpaCustomerRepository` (Implementation).
-   **Immutability**: Prefer immutable structures. Use `List.of()` and Records.
-   **Dependency Injection**: Use constructor-based injection for mandatory dependencies.

## 🛠️ Project Templates

- **Maven Standalone POM**: Modern `pom.xml` for single-module projects.
  `/./templates/pom.xml`
- **Maven Parent POM**: Multi-module parent configuration.
  `./templates/pom-parent.xml`
- **Maven Module POM**: Individual module configuration inheriting from parent.
  `./templates/pom-module.xml`

## 📂 Standard Project Structure
```
src/
  main/java/com/app/
    enterprise/       # Entities (Inmost Layer)
    application/      # Use Cases
    interface/        # Controllers, Mappers, DTOs
    infrastructure/   # DB Config, Implementations (Outermost Layer)
  test/groovy/        # Spock Specifications (Testing Layer)
```

---
Using this skill ensures Java 25 applications are built with the highest standards of performance, maintainability, and testing excellence.
