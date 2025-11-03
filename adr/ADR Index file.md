# 🧭 Architecture Decision Record (ADR) Index  
### Direct Debit Mandate Update Microservice  
*Version: 1.0 — Generated 2025-11-03*

This index catalogues all **Architecture Decision Records (ADRs)** governing the Direct Debit Mandate Update microservice.  
Each ADR is linked to its enforcing **Policies-as-Code** and **Controls-as-Code** artefacts, ensuring full traceability and compliance in the Architecture-as-Code (AaC) model.

---

## 🧩 ADR Catalogue Overview

| ADR ID | Title | Status | Linked Policies | Linked Controls | Summary |
| ------ | ------ | ------- | ---------------- | ---------------- | -------- |
| **ADR-001** | [API Exposure via Amazon API Gateway](./ADR-001-api-exposure.md) | ✅ *Accepted* | `paC/api-authentication.rego`, `paC/api-versioning.rego`, `paC/api-security.rego` | `INFRA-002-control`, `SEC-001-control`, `SEC-004-control`, `ARC-006-control` | Standardises ingress through API Gateway enforcing HTTPS, OAuth2, and versioned endpoints. |
| **ADR-002** | [Secrets and Credentials Management](./ADR-002-secrets-management.md) | ✅ *Accepted* | `ARC-POLICY-104-control`, `ARC-POLICY-105-control`, `ARC-POLICY-106-control` | `SEC-003-control`, `INFRA-004-control` | All secrets must be sourced from AWS Secrets Manager; no hardcoded credentials permitted. |
| **ADR-003** | [Integration with Thought Machine Vault Core via Adapter Pattern](./ADR-003-vault-adapter.md) | ✅ *Accepted* | `ARC-POLICY-103-control`, `ARC-POLICY-109-control` | `ARC-002-control`, `ARC-003-control`, `ARC-007-control` | All Vault Core interactions occur via a named adapter to enforce decoupling and testability. |
| **ADR-004** | [Audit Logging and Observability](./ADR-004-audit-observability.md) | ✅ *Accepted* | `ARC-POLICY-107-control`, `ARC-POLICY-108-control` | `INFRA-003-control`, `INFRA-005-control`, `SEC-005-control` | Defines standard for structured audit logs and CloudWatch observability integration. |
| **ADR-005** | [Runtime Environment on AWS Fargate](./ADR-005-runtime-fargate.md) | ✅ *Accepted* | `ARC-POLICY-101-control` | `INFRA-001-control`, `INFRA-006-control` | Mandates deployment on ECS Fargate with tfsec-enforced IaC compliance. |
| **ADR-006** | [Secure Layered Architecture](./ADR-006-layered-architecture.md) | ✅ *Accepted* | `ARC-POLICY-109-control` | `ARC-001-control`, `ARC-002-control`, `ARC-003-control` | Enforces Controller → Service → Adapter architecture layering and clean dependency boundaries. |
| **ADR-007** | [Diagram Governance and Readability](./ADR-007-diagram-governance.md) | ✅ *Accepted* | `ARC-POLICY-110-control` | `ARC-007-control` | Establishes consistent diagram standards including legend, skinparam, and metadata annotations. |

---

## 📚 Governance Traceability

| Layer | Description | Examples |
| ------ | ------------ | -------- |
| **ADR (Architecture Decision Record)** | Captures key architectural decisions and rationale for the Direct Debit Mandate Update microservice. | `ADR-001` through `ADR-007` |
| **Policy-as-Code (PaC)** | Declarative governance logic enforcing architecture design patterns and platform alignment. | `ARC-POLICY-101-control`, `ARC-POLICY-104-control` |
| **Control-as-Code (CaC)** | Concrete, testable assertions enforcing runtime compliance and design integrity. | `SEC-001-control`, `INFRA-003-control`, `ARC-002-control` |

All ADRs form part of the **continuous governance pipeline**, automatically validated during Pull Requests via GitHub Actions and OPA policy checks.

---

## ⚙️ Validation & Automation Path

| Stage | Tool | Description |
| ------ | ---- | ------------ |
| **Design-time Validation** | OPA (Rego) | Enforces architectural patterns (API Gateway, Secrets, Layering) before merge. |
| **Static Analysis** | ArchUnit / tfsec | Validates source code structure and IaC configuration. |
| **Runtime Conformance** | CloudWatch / Drift Detector | Verifies deployed state matches ADR and policy declarations. |
| **Governance Reporting** | Backstage / Helio Dashboards | Visualises alignment, risk posture, and compliance coverage. |

---

## 🧮 Version Control & Supersession

All ADRs include a `supersedes` and `superseded_by` field to support continuous evolution:
- Updates to design patterns or enforcement methods create a **new ADR version** (e.g., ADR-001-v2).  
- Superseded ADRs remain immutable for historical traceability.  
- The `ADR-INDEX.md` file is regenerated automatically at each tagged release.

---

## 🏁 Summary

This index provides a **single navigable view** of all architecture decisions, governance artefacts, and compliance hooks related to the Direct Debit Mandate Update service.  
Together, these ADRs establish the baseline for **consistent, auditable, and enforceable** architecture across your ValueOps and Architecture-as-Code domains.

---

_Last updated: 2025-11-03 by Architecture Governance Automation_
