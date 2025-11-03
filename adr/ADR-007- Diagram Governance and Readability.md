---
id: ADR-007
title: Diagram Governance and Readability
status: Accepted
date: 2025-11-03
context: Direct Debit Mandate Update Microservice
linkedPolicies:
  - ARC-POLICY-110-control
linkedControls:
  - ARC-007-control
tags: [architecture, governance, diagrams, plantuml, readability, compliance, directdebit]
supersedes: null
---

## 🧭 Context
Architecture diagrams are the **source of truth** for understanding system design, compliance posture, and runtime topology in the Architecture-as-Code model.  

Historically, PlantUML diagrams across teams varied in format and readability, leading to:
- Inconsistent visual styles and missing legends.  
- Ambiguity in interpreting system boundaries or component roles.  
- Difficulty for auditors and engineers to verify compliance annotations (e.g., `<<fargate>>`, `<<adapter>>`, `<<observability>>`).  

To enable **machine validation**, **human comprehension**, and **automated governance**, all diagrams must adhere to a **consistent structure, annotation style, and visual format**.

---

## 💡 Decision
All architecture diagrams (.puml files) representing the Direct Debit Mandate Update microservice must:
1. Include a **`legend`** section explaining icons, stereotypes, and symbols.  
2. Include a **`skinparam`** block defining visual standards (font, colors, line styles).  
3. Follow the enterprise standard for PlantUML diagram components, including mandatory stereotypes such as `<<fargate>>`, `<<service>>`, `<<adapter>>`, and `<<observability>>`.  
4. Use **clear naming conventions** aligned to code namespaces and repository structure.  
5. Include all defined **System_Boundary** elements that match the runtime deployment context (e.g., Fargate, API Gateway).  
6. Be version-controlled alongside code, policies, and ADRs to support traceability in GitHub.  

These standards ensure diagrams are **readable by humans** and **validated automatically** by CI/CD pipelines during pre-merge checks.

---

## ⚙️ Implementation & Enforcement

### Enforcement Mechanisms
- **Policies-as-Code (PaC):**
  - `ARC-POLICY-110-control` – Each PlantUML diagram must include a legend and skinparam block.  

- **Controls-as-Code (CaC):**
  - `ARC-007-control` – All classes must belong to allowed package namespaces to enforce diagram-to-code consistency.  

### Validation Methods
- **OPA/Rego rules** validate that each `.puml` file includes a legend, skinparam, and mandatory annotations (`<<fargate>>`, `<<controller>>`, etc.).  
- **Pre-commit hooks** in GitHub verify that every new or modified diagram conforms to the enterprise syntax pattern.  
- **Static schema checks** ensure PlantUML metadata tags (`@domain`, `@policy`, `@control`) are included in the diagram front matter.  
- **Continuous validation pipelines** cross-reference diagram component names with package names in source code and metadata definitions.  

---

## 📈 Consequences
**Positive:**
- Improves consistency and clarity of architectural documentation.  
- Enables automated compliance and drift detection between design and implementation.  
- Reduces review overhead by making diagrams self-explanatory.  
- Enhances stakeholder confidence and supports audit readiness.  

**Negative / Trade-offs:**
- Slight increase in authoring time to include mandatory metadata blocks.  
- Requires developer familiarity with PlantUML formatting standards.  

---

## 🧩 Example Diagram Header Template

```plantuml
@startuml DirectDebitMandateUpdate
!include <C4/C4_Container.puml>

skinparam backgroundColor #FFFFFF
skinparam componentStyle rectangle
skinparam defaultFontName Arial

legend left
  <&server> Fargate Service
  <&cloud> API Gateway
  <&database> RDS Audit Store
  <&eye> CloudWatch Observability
endlegend
