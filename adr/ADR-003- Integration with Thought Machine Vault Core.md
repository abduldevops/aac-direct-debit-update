---
id: ADR-003
title: Integration with Thought Machine Vault Core via Adapter Pattern
status: Accepted
date: 2025-11-03
context: Direct Debit Mandate Update Microservice
linkedPolicies:
  - ARC-POLICY-103-control
  - ARC-POLICY-109-control
linkedControls:
  - ARC-002-control
  - ARC-003-control
  - ARC-007-control
tags: [architecture, adapter, integration, vaultcore, aws, directdebit]
supersedes: null
---

## 🧩 Context
The Direct Debit Mandate Update microservice must communicate securely with **Thought Machine Vault Core** to create, update, and verify direct-debit mandates.  
Direct coupling of the domain service to the external Vault Core API introduces several risks:
- Tight integration between business logic and external provider APIs.  
- Increased complexity for testing and mocking.  
- Higher likelihood of vendor lock-in and brittle dependencies.  
- Reduced clarity in failure isolation and error propagation.  

To maintain clean architectural boundaries and align with the organisation’s **Domain-Driven Design (DDD)** and **Architecture-as-Code** standards, all Vault Core interactions will be abstracted behind a dedicated **adapter layer**.

---

## 💡 Decision
All communication with **Thought Machine Vault Core** must occur through a defined **adapter component** (`VaultCoreClientAdapter`) annotated as `<<adapter>>` in PlantUML diagrams.  

### Structural requirements
1. Controllers delegate to a **Service** class for business rules (`<<service>>`).  
2. The service invokes the **Adapter** to call Vault Core’s APIs.  
3. The Adapter manages authentication, request translation, error handling, and retries.  
4. The Adapter must be the only component importing Vault Core SDKs or API client code.  
5. The architecture diagram must show this boundary within a `System_Boundary` representing the **Fargate runtime**, with `<<controller>>`, `<<service>>`, and `<<adapter>>` elements clearly defined.  

This pattern enforces testability, loose coupling, and consistent alignment with AWS cloud-native design principles.

---

## ⚙️ Implementation & Enforcement

### Enforcement Mechanisms
- **Policies-as-Code (PaC):**
  - `ARC-POLICY-103-control` – Vault Core must only be accessed via a named adapter container.  
  - `ARC-POLICY-109-control` – Every diagram must contain at least one controller-service pair within a boundary.

- **Controls-as-Code (CaC):**
  - `ARC-002-control` – Repositories must not be injected directly into controllers.  
  - `ARC-003-control` – Domain entities must not depend on controller or DTO packages.  
  - `ARC-007-control` – All classes must reside in approved package namespaces (e.g., `com.company.directdebitupdate.*`).  

### Validation Methods
- **ArchUnit** tests verify layer dependencies (`controller → service → adapter`) and detect direct API client usage in domain logic.  
- **OPA/Rego** checks validate that PlantUML diagrams contain a `<<adapter>>` element connecting to the Vault Core system boundary.  
- **GitHub Actions** run automated compliance pipelines enforcing both static (code) and structural (diagram) integrity.

---

## 📈 Consequences
**Positive:**
- Strong separation of concerns, enabling independent testing and mock-based integration.  
- Simplified migration path if Vault Core APIs evolve or provider changes.  
- Greater maintainability and observability through a single integration point.  
- Enhanced alignment with enterprise-wide layering and modularity standards.

**Negative / Trade-offs:**
- Slight increase in initial implementation effort and boilerplate.  
- Requires additional logging and tracing between service and adapter layers.  

---

## 🧭 Status & Next Steps
- Applies to all current and future Vault-integrated microservices.  
- Next step: extend adapter telemetry with **structured logging** and **CloudWatch correlation IDs** (to be captured under ADR-004).  
- Review adherence quarterly via Architecture Assurance Board.

---

## 🔗 Traceability
| Type | Reference | Description |
|------|------------|-------------|
| **Policy** | `ARC-POLICY-103-control` | Restricts Vault Core access to a named adapter container. |
|  | `ARC-POLICY-109-control` | Ensures layered architecture (`controller–service–adapter`). |
| **Control** | `ARC-002-control` | Prohibits repository injection into controllers. |
|  | `ARC-003-control` | Enforces clean domain separation. |
|  | `ARC-007-control` | Ensures classes follow approved namespace structure. |

---

## 📚 Supersession Chain
- **Supersedes:** *None*  
- **Superseded by:** Future ADRs covering multi-adapter orchestration or domain event integration for Vault Core.

---
