
# 🧭 Architecture Decision Records (ADRs)
### Direct Debit Mandate Update Microservice

---

## 📘 Purpose

This directory contains all **Architecture Decision Records (ADRs)** that define the key design and governance decisions for the **Direct Debit Mandate Update Microservice**.  
Each ADR is a **living, version-controlled artefact** — expressing architectural intent, rationale, and enforcement through automation.

ADRs operate as the **top layer** of the **Architecture-as-Code (AaC)** model.  
They are directly linked to executable artefacts:
- **Policies-as-Code (PaC)** — Rego-based rules for governance validation.  
- **Controls-as-Code (CaC)** — Testable rules (ArchUnit, tfsec, OPA) for runtime and build-time enforcement.

---

## 🧩 Repository Structure

```

/microservice/
├── adrs/
│    ├── ADR-001-api-exposure.md
│    ├── ADR-002-secrets-management.md
│    ├── ADR-003-vault-adapter.md
│    ├── ADR-004-audit-observability.md
│    ├── ADR-005-runtime-fargate.md
│    ├── ADR-006-layered-architecture.md
│    ├── ADR-007-diagram-governance.md
│    ├── ADR-INDEX.md
│    └── README.md
├── policies/
│    └── *.rego
└── controls/
└── *.tf / *.java / *.rego

````

| Artefact Type | Purpose | Validation Method |
| -------------- | -------- | ----------------- |
| **ADR** | Captures key architecture decisions and rationale. | CI validation via OPA + human review. |
| **Policy-as-Code (PaC)** | Encodes governance rules as Rego policies. | OPA test runner in GitHub Actions. |
| **Control-as-Code (CaC)** | Implements technical enforcement (e.g., ArchUnit, tfsec). | Static/dynamic validation in CI/CD. |

Each ADR explicitly references its supporting PaC and CaC artefacts, ensuring **full traceability from intent to enforcement**.

---

## ⚙️ Governance Automation Flow

| Stage | Tool | Description |
| ------ | ---- | ------------ |
| **Design-Time** | OPA (Rego) | Validates that architecture diagrams and ADR metadata comply with defined patterns (API Gateway, Fargate, Secrets Manager). |
| **Static Analysis** | ArchUnit / tfsec | Checks Java layering and Terraform compliance. |
| **CI/CD Enforcement** | GitHub Actions | Runs pre-merge ADR, policy, and control validation pipelines. |
| **Runtime Assurance** | Drift Detector / CloudWatch | Confirms deployed environment aligns with ADR and policy declarations. |
| **Reporting & Dashboards** | Backstage / Helio | Displays live architecture compliance posture. |

This continuous loop ensures decisions are **not only documented but actively enforced**.

---

## 🧠 Authoring a New ADR

To add a new ADR:

1. **Copy the template**:  
   ```bash
   cp ./adrs/ADR-TEMPLATE.md ./adrs/ADR-008-new-decision.md
````

2. Update the YAML front matter:

   * `id`: Next incremental ADR number.
   * `title`: Short, descriptive title.
   * `linkedPolicies` and `linkedControls`: IDs of corresponding enforcement artefacts.
   * `status`: `Proposed` → `Accepted` (after review).
3. Complete the sections:

   * **Context** – Problem or rationale.
   * **Decision** – What has been decided.
   * **Implementation & Enforcement** – How policies/controls validate it.
   * **Consequences** – Benefits, trade-offs, risks.
   * **Traceability** – Table of PaC and CaC artefacts.
   * **Supersession Chain** – Tracks replaced or future ADRs.
4. Commit with a meaningful message:

   ```bash
   git add adrs/ADR-008-new-decision.md
   git commit -m "Add ADR-008: New architectural decision"
   git push
   ```

---

## 🔗 Traceability Example

| Layer               | Example Reference               | Description                                                               |
| ------------------- | ------------------------------- | ------------------------------------------------------------------------- |
| **ADR**             | `ADR-002-secrets-management.md` | Defines use of AWS Secrets Manager.                                       |
| **Policy-as-Code**  | `ARC-POLICY-106-control`        | Requires declared dependency on Secrets Manager in architecture diagrams. |
| **Control-as-Code** | `INFRA-004-control`             | Ensures secrets are stored securely in AWS Secrets Manager.               |

This traceability allows automated cross-validation across code, architecture diagrams, and infrastructure.

---

## 🧾 Versioning & Supersession

* Each ADR is **immutable** once marked *Accepted*.
* Subsequent updates must create a **new ADR** (e.g., `ADR-001-v2`) referencing the original in the `supersedes` field.
* Superseded ADRs remain part of the repository for **audit and lineage**.

Example:

```yaml
supersedes: ADR-001
```

---

## 🧮 Local Validation Commands

Developers can validate architecture conformance locally before committing:

```bash
# Run OPA policy tests
opa test ./policies --verbose

# Run tfsec to validate Terraform security compliance
tfsec ./infra

# Run ArchUnit tests for Java package structure
mvn test -Dtest=ArchUnit*

# Validate PlantUML diagrams
make validate-diagrams
```

---

## ✅ Review & Approval Process

| Stage      | Reviewer                        | Action                                                   |
| ---------- | ------------------------------- | -------------------------------------------------------- |
| Draft      | Author / Domain Architect       | Creates ADR and commits initial version.                 |
| Review     | Architecture Review Board (ARB) | Validates context, enforcement mapping, and consistency. |
| Acceptance | ARB Approval                    | Marks ADR as *Accepted*; triggers automated indexing.    |
| Compliance | CI/CD Pipelines                 | Enforce linked PaC and CaC artefacts.                    |

---

## 🧩 Continuous Indexing

The [`ADR-INDEX.md`](./ADR-INDEX.md) file is automatically regenerated to maintain:

* ADR metadata (status, linked controls, policies).
* Traceability for compliance dashboards.
* Cross-reference to PlantUML architecture artefacts and test coverage.

---

## 📚 Related Files

| File                                   | Description                                                   |
| -------------------------------------- | ------------------------------------------------------------- |
| [`ADR-INDEX.md`](./ADR-INDEX.md)       | Catalogue of all ADRs and linked enforcement artefacts.       |
| [`ADR-TEMPLATE.md`](./ADR-TEMPLATE.md) | Template for creating new ADRs.                               |
| [`/policies/`](../policies/)           | Directory containing OPA/Rego Policies-as-Code.               |
| [`/controls/`](../controls/)           | Directory containing Controls-as-Code (tfsec, ArchUnit, OPA). |

---

## 🏁 Summary

ADRs serve as **enforceable architecture artefacts**, not just documentation.
By linking each decision to executable governance layers — policies and controls — this repository achieves **continuous architectural alignment** across design, code, and runtime.

> “Architecture that governs itself” — Architecture-as-Code.

---

*Last updated: 2025-11-03 by Architecture Governance Automation*

```
