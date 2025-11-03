---
id: ADR-XXX
title: <Short, Descriptive Title>
status: Proposed
date: YYYY-MM-DD
context: Direct Debit Mandate Update Microservice
linkedPolicies:
  - <policy-id-1>
  - <policy-id-2>
linkedControls:
  - <control-id-1>
  - <control-id-2>
tags: [architecture, governance, placeholder, update]
supersedes: null
---

# 🧭 Architecture Decision Record Template
> Use this template to create new ADRs for the Direct Debit Mandate Update microservice.  
> Each ADR must represent a **single architectural decision** that can be linked to testable Policies-as-Code (PaC) and Controls-as-Code (CaC).

---

## 🧩 Context
Describe the background and reasoning behind the decision.  
Include:
- The business or technical problem this decision addresses.  
- Relevant constraints, risks, or dependencies.  
- Reference any prior ADRs or architectural discussions that influenced this decision.

Example:
> Previous services exposed unsecured endpoints and lacked versioning.  
> To align with the Architecture-as-Code model, this ADR defines a standard API exposure approach using AWS API Gateway.

---

## 💡 Decision
Clearly state **what has been decided**.  
Include:
- The architectural pattern or approach being adopted.  
- Mandatory components, configurations, or annotations (e.g., `<<fargate>>`, `<<adapter>>`).  
- Any boundaries, design rules, or required technologies.

Example:
> All external-facing REST APIs will be exposed via Amazon API Gateway using HTTPS and OAuth2 authentication.

---

## ⚙️ Implementation & Enforcement
Describe **how** the decision will be implemented and enforced.

### Enforcement Mechanisms
List associated **Policies-as-Code (PaC)** and **Controls-as-Code (CaC)** that verify or enforce the decision.

Example:
- **Policies-as-Code:**
  - `ARC-POLICY-102-control` – Enforces use of `<<api_gateway>>` container in PlantUML.
  - `ARC-POLICY-109-control` – Ensures diagram includes controller–service pairs.
- **Controls-as-Code:**
  - `INFRA-002-control` – API Gateway must use HTTPS-only endpoints.
  - `SEC-001-control` – All REST endpoints require authentication.

### Validation Methods
Explain which tools or pipelines will validate conformance:
- **OPA/Rego** for architecture rule validation.
- **ArchUnit** for Java package structure and layering tests.
- **tfsec** for Terraform configuration checks.
- **GitHub Actions** for CI/CD enforcement.

---

## 📈 Consequences
Summarize both the **benefits** and **trade-offs** of this decision.

**Positive:**
- Improves consistency and governance.
- Reduces security risks.
- Enables automation of compliance checks.

**Negative / Trade-offs:**
- Introduces additional complexity or overhead.
- Requires new tooling, IAM configuration, or maintenance effort.

---

## 🧭 Status & Next Steps
State the current adoption state (e.g., *Proposed*, *Accepted*, *Deprecated*) and outline future actions.

Example:
> Applied to all microservices in the Payments domain.  
> Future ADRs will extend this decision to support multi-region or API throttling strategies.

---

## 🔗 Traceability
Provide a traceable mapping of linked Policies and Controls for audit and compliance.

| Type | Reference | Description |
|------|------------|-------------|
| **Policy** | `<policy-id>` | Brief description of the governance rule. |
| **Control** | `<control-id>` | Brief description of the implementation enforcement. |

---

## 📚 Supersession Chain
- **Supersedes:** *<prior-ADR-if-any>*  
- **Superseded by:** *<future-ADR-if-any>*  

> Each ADR must remain immutable once accepted.  
> Revisions are introduced as a new ADR (e.g., ADR-008-v2) linked via this chain.

---

## ✅ Review Checklist
Before submitting a new ADR:
- [ ] Includes YAML front matter with IDs, policies, and controls.  
- [ ] Uses clear, unambiguous language.  
- [ ] Includes a corresponding PlantUML diagram update.  
- [ ] Validated through OPA/Rego, ArchUnit, and tfsec in CI.  
- [ ] Reviewed by Domain Architect or ARB before acceptance.  

---

_Last updated: 2025-11-03 by Architecture Governance Automation_
