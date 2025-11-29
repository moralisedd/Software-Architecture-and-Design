---
status: {Accepted}
date: {2025-11-29}
decision-makers: {Chief, Architect & Leader Develop - David A.}
consulted: {System Administrator, Help Desk Manager, Security Specialist}
informed: {All Project Stakeholders}
---

# ADR-001: Adoption of Monolithic Layered Architecture

## Context and Problem Statement

We are designing a Complaint Management System (CMS) for ABC Limited, targeting large Banking and Telecom clients. The system must handle high volume (20 million customers, 10% yearly growth) and enforce strict multi-tenancy to ensure data isolation between corporate clients (e.g., NatWest and Barclays data must be separate).

We need to select the foundational Architectural Style that provides the best balance of structure, maintainability, and scalability to meet these Non-Functional Requirements (NFRs) while facilitating a focused initial development effort.

## Decision Drivers

* (Maintainability & Separation of Concerns:) The architecture must enforce clean boundaries between the UI, business logic, and data, as recommended for layered architectures.

* (Time-to-Market & Complexity:) We must select an approach that minimizes operational overhead and speeds up the initial development phase, aligning with the objective of "getting funding from venture capitalists with the outcome".

* (Transactional Integrity:) The core function of "registering a problem" requires strong ACID (Atomicity, Consistency, Isolation, Durability) guarantees, favoring a single data store approach initially.

* (Deployment Simplicity:) A single, unified deployment unit is preferred to the complexity of managing multiple services in a distributed environment at this stage.

## Considered Options

* Monolithic Layered Architecture

* Microservices Architecture (Distributed)

* Three-Tier Architecture (Included as it is a common refinement of Layered)

## Decision Outcome

Chosen option: ("Monolithic Layered Architecture") (specifically, a Three-Tier refinement), because it provides the best trade-off for the initial phase of the CMS project. It offers a clear, well-understood structure that facilitates maintainability and clear separation of concerns, which is ("excellent for separation of concerns and maintainability").

While a (Microservices) approach offers superior long-term scalability, the complexity of managing distributed transactions and deployment overhead is a significant risk that could delay time-to-market. The (Layered Monolith) allows us to build the core transactional system efficiently while deferring the decision to refactor into Microservices until performance scaling demands it.

### Consequences

* Good: Strong separation of concerns ensures that business logic (e.g., resolving complex problems) is isolated from the UI layer.

* Good: Simplifies development, testing, and deployment efforts (a single artifact).

* Bad: Scaling is "all-or-nothing"—the entire application must be scaled to handle the high user load, which could become inefficient at extreme volumes.

* Neutral: Performance may suffer from "performance overhead (too many layers)" if data access is poorly managed.

### Confirmation

Compliance will be confirmed during the Proof of Concept (PoC). The code base must be structured with explicit packages/modules for the Presentation, Business Logic, and Data Access layers. Code reviews will ensure that there are no "back-door" calls that bypass the Business Logic layer.

## Pros and Cons of the Options

### Monolithic Layered Architecture

A well-known structure where components are organized into horizontal, sequential layers (Presentation -> Business Logic -> Data Access).

* Good, because it enforces modularity and strong separation, which aids in long-term maintainability.

* Good, because it is less complex to develop and test than distributed systems.

* Good, because changes to the UI layer (e.g., compliance with WCAG 2) can be made without impacting core business logic.

* Bad, because the entire system is tightly coupled; a failure in one area can bring down the entire application (affecting 24/7 Availability).

### Microservices Architecture

A distributed style where the system is split into small, independent, deployable services.

* Good, because it provides superior Scalability and Availability by allowing individual components to be scaled and deployed independently.

* Bad, because it dramatically increases complexity due to the need for inter-service communication, distributed tracing, and data consistency management.

* Bad, because it increases the time and cost required for the initial development phase, potentially undermining the funding objective.

## More Information

### Three-Tier Architecture
A refinement of Layered architecture that explicitly separates the Presentation Tier, Application (Business Logic) Tier, and Data Tier, often deployed on separate machines.

* Good, because it offers better distribution of resources than a simple two-tier monolith, providing a middle ground for managing the "20 million customers" scale.

* Neutral, because it still uses a single code base (the Application Tier), retaining the simplicity and coupling risks of a monolith.