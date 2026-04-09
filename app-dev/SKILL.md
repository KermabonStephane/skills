---
name: Application Development (Clean & Hexagonal Architecture)
description: Guidelines and templates for building maintainable applications using Clean/Hexagonal architecture, Clean Code principles, and standardized documentation (ADRs, functional/technical docs).
---

# Application Development Guide

This skill defines the standards for architecting, coding, and documenting modern software applications. It focuses on long-term maintainability through decoupling and clear documentation.

## 🏗️ Architectural Patterns

### 1. Clean Architecture / Hexagonal Architecture
The goal is to protect the core business logic (Domain) from external changes (Database, UI, Frameworks).

-   **The Domain (Entities & Value Objects)**: The heart of the application. Pure logic, no dependencies.
-   **The Application (Use Cases/Ports)**: Orchestrates business rules. Defines "Ports" (Interfaces) for external interaction.
-   **The Infrastructure (Adapters)**: Implements the Ports. Contains technology-specific code (Persistence, Web, Messaging).
-   **The Interface (Controllers/Presenters)**: Entry points for the application.

**The Dependency Rule**: Dependencies must only point inwards.

## ✍️ Clean Code Principles

-   **Small Functions**: Keep functions under 20 lines. Do one thing well.
-   **Meaningful Names**: Descriptive, searchable, and intent-revealing names for classes, methods, and variables.
-   **No Side Effects**: Functions should do only what they promise.
-   **DRY (Don't Repeat Yourself)**: Abstract common logic into reusable components.
-   **Boy Scout Rule**: Always leave the code cleaner than you found it.

## 📄 Documentation Standards

Every project MUST have a standardized documentation structure.

### 1. README.md
The entry point. Must contain:
-   Project description.
-   Getting started guide.
-   Links to **Functional** and **Technical** documentation.

### 2. Functional Documentation (`docs/functional/`)
Describes *what* the application does from a business perspective.
-   User Personas.
-   Business Rules.
-   User Journeys.

### 3. Technical Documentation (`docs/technical/`)
Describes *how* the application is built.
-   Architecture Diagrams.
-   Data Schema.
-   Integration Points.

### 4. Architecture Decision Records (ADR) (`docs/adr/`)
A log of significant architectural decisions made during the project.
-   **Format**: Title, Date, Context, Decision, Consequences.

## 🛠️ Templates

This skill provides ready-to-use templates:

-   **[README.md Template](./templates/README.md)**
-   **[Functional Doc Template](./templates/FUNCTIONAL.md)**
-   **[Technical Doc Template](./templates/TECHNICAL.md)**
-   **[ADR Template](./templates/ADR_TEMPLATE.md)**

---
Applying this skill ensures that applications are not only "built right" (Code Quality) but also "well documented" (Knowledge Sharing) and "properly designed" (Architecture).
