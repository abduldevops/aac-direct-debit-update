---
id: ADR-006
title: Secure Layered Architecture
status: Accepted
date: 2025-11-03
context: Direct Debit Mandate Update Microservice
linkedPolicies:
  - ARC-POLICY-109-control
linkedControls:
  - ARC-001-control
  - ARC-002-control
  - ARC-003-control
tags: [architecture, layering, design, security, maintainability, directdebit]
supersedes: null
---

## 🧱 Context
The Direct Debit Mandate Update microservice must enforce a consistent **three-layer architecture** to maintain modularity, testability, and security.  
Earlier systems suffered from:
- Business logic embedded in controllers.  
- Repositories and persistence logic tightly coupled to presentation layers.  
- Inconsistent exception handling and dependency management.  

These issues increased technical debt, weakened separation of concerns, and introduced the risk of data exposure or unintended cross-layer dependencies.  

To comply with the enterprise **Architecture-as-Code (AaC)** governance model, all microservices must adhere to a layered architecture that clearly separates responsibilities and aligns with Domain-Driven Design principles.

---

## 💡 Decision
All components in the Direct Debit Mandate Update microservice must follow a **Controller → Service → Adapter/Repository** layering structure.

### Design Rules
1. **Controllers (`<<controller>>`)**
   - Handle HTTP requests and responses only.  
   - Map to DTOs for inbound/outbound data transformation.  
   - Delegate all business logic to services.  
2. **Services (`<<service>>`)**
   - Contain domain logic and orchestrate interactions with adapters.  
   - Must not depend directly on presentation or persistence layers.  
3. **Adapters / Repositories (`<<adapter>>`, `<<repository>>`)**
   - Handle external API calls or database operations.  
   - Must not expose internal entities directly to services or controllers.  
4. **Cross-Layer Rules**
   - Domain entities must not depend on DTO or controller packages.  
   - Each layer must reside in clearly defined package namespaces (e.g., `com.company.directdebitupdate.api`, `.service`, `.adapter`).  
   - All exceptions must be centralized through a global `@ControllerAdvice` handler.

PlantUML architecture diagrams must include, within each `System_Boundary`, at least one `<<controller>>` and `<<service>>` component to reflect this layering model.

---

## ⚙️ Implementation & Enforcement

### Enforcement Mechanisms
- **Policies-as-Code (PaC):**
  - `ARC-POLICY-109-control` – Every architecture diagram must include a controller–service pair within a defined boundary.  

- **Controls-as-Code (CaC):**
  - `ARC-001-control` – DTOs must be used for all controller inputs/outputs.  
  - `ARC-002-control` – Repositories must not be injected into controllers.  
  - `ARC-003-control` – Domain entities must not depend on controller or DTO packages.  

### Validation Methods
- **ArchUnit** layer tests validate package dependencies (`controller → service → adapter`) and enforce separation of concerns.  
- **OPA/Rego** diagram rules confirm presence of `<<controller>>` and `<<service>>` annotations.  
- **GitHub Actions CI** runs pre-merge checks for layering violations.  
- **Static code analysis** and IDE linters ensure no cross-layer imports or circular dependencies.  

---

## 📈 Consequences
**Positive:**
- Clear separation of concerns improving maintainability and readability.  
- Reduced security risk by limiting access between presentation and persistence layers.  
- Easier unit testing and mocking of isolated components.  
- Improved traceability and architectural conformance across the enterprise.  

**Negative / Trade-offs:**
- Slight increase in boilerplate code (DTO mapping, service orchestration).  
- Requires developer discipline to maintain package conventions.  

---

## 🧭 Status & Next Steps
- Applies to all existing and new microservices in the **Direct Debit** and **Payments** domain.  
- Integrate layer dependency enforcement into IDE templates and project scaffolding.  
- Future ADRs (e.g., ADR-010 on **Cross-Domain Contracts**) may extend or supersede this model to include domain event patterns.

---

## 🔗 Traceability
| Type | Reference | Description |
|------|------------|-------------|
| **Policy** | `ARC-POLICY-109-control` | Enforces inclusion of `<<controller>>` and `<<service>>` components per architecture boundary. |
| **Control** | `ARC-001-control` | DTOs required for controller inputs/outputs. |
|  | `ARC-002-control` | Prevents repository injection into controllers. |
|  | `ARC-003-control` | Blocks domain dependencies on presentation or DTO layers. |

---

## 📚 Supersession Chain
- **Supersedes:** *None*  
- **Superseded by:** Future ADRs extending domain event-based layering or cross-cutting architecture patterns.

---
