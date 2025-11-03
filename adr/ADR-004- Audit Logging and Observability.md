---
id: ADR-004
title: Audit Logging and Observability
status: Accepted
date: 2025-11-03
context: Direct Debit Mandate Update Microservice
linkedPolicies:
  - ARC-POLICY-107-control
  - ARC-POLICY-108-control
linkedControls:
  - INFRA-003-control
  - INFRA-005-control
  - SEC-005-control
tags: [audit, observability, logging, aws, cloudwatch, compliance, directdebit]
supersedes: null
---

## 🧮 Context
The Direct Debit Mandate Update microservice handles financial transactions that must be **traceable, auditable, and observable**.  
Every request to create or update a mandate involves customer data, system integrations, and regulatory requirements under **GDPR** and **FCA**.

Historically, audit and monitoring were treated as post-deployment add-ons, leading to:
- Inconsistent audit record formats.  
- Limited correlation between API calls, service events, and infrastructure metrics.  
- Gaps in traceability when investigating customer or operational incidents.  

To comply with organisational governance and deliver full observability under the **Architecture-as-Code (AaC)** model, all audit and monitoring mechanisms must be defined, validated, and deployed **as code** alongside the service.

---

## 💡 Decision
All operations within the Direct Debit Mandate Update microservice must:
1. **Generate structured audit events** for all critical business operations (create/update).  
2. **Store all audit records** in an encrypted **Amazon RDS PostgreSQL** instance.  
3. **Emit operational telemetry** (metrics, logs, traces) to **Amazon CloudWatch**.  
4. **Include a `<<observability>>` container** in the PlantUML architecture diagram representing CloudWatch integration.  
5. **Enforce consistent trace identifiers** across API Gateway, Fargate containers, and downstream components for end-to-end correlation.  
6. **Mask all PII fields** in audit logs while preserving data lineage and timestamps.  

The audit log schema and telemetry output will form part of the version-controlled architecture artefacts to ensure automated validation and traceability.

---

## ⚙️ Implementation & Enforcement

### Enforcement Mechanisms
- **Policies-as-Code (PaC):**
  - `ARC-POLICY-107-control` – Stateful services must store logs or data in defined data store containers.  
  - `ARC-POLICY-108-control` – Diagrams must include a `<<observability>>` container or reference CloudWatch.  

- **Controls-as-Code (CaC):**
  - `INFRA-003-control` – Audit logs must be stored in encrypted RDS.  
  - `INFRA-005-control` – CloudWatch must capture logs and custom metrics.  
  - `SEC-005-control` – Mandatory logging of all failed login or API attempts.  

### Validation Methods
- **OPA/Rego** rules confirm presence of observability containers and links in `.puml` diagrams.  
- **tfsec** ensures RDS encryption at rest and CloudWatch log groups with encryption enabled.  
- **ArchUnit** checks verify audit service class structure and log masking annotations.  
- **GitHub Actions CI** executes automated compliance pipelines ensuring all logging and audit controls pass before merge or deployment.

---

## 📈 Consequences
**Positive:**
- Enables full forensic traceability for financial and compliance audits.  
- Supports rapid incident response with unified telemetry (metrics, traces, logs).  
- Ensures proactive monitoring and alerting for latency or error thresholds.  
- Reduces regulatory and operational risk through continuous audit evidence capture.  

**Negative / Trade-offs:**
- Slight increase in infrastructure cost (CloudWatch + RDS storage).  
- Development effort required for consistent audit schema and correlation ID propagation.  

---

## 🧭 Status & Next Steps
- Applicable to all **Direct Debit** domain microservices.  
- Extend audit schema to include request IDs, OAuth2 client ID, and mandate lifecycle event types.  
- Future ADRs (e.g., ADR-009 on **Centralised Audit Schema Governance**) will expand multi-service observability standards.

---

## 🔗 Traceability
| Type | Reference | Description |
|------|------------|-------------|
| **Policy** | `ARC-POLICY-107-control` | Enforces use of defined log and data store containers. |
|  | `ARC-POLICY-108-control` | Requires inclusion of `<<observability>>` or CloudWatch in diagrams. |
| **Control** | `INFRA-003-control` | Audit logs stored in encrypted RDS. |
|  | `INFRA-005-control` | CloudWatch must capture logs and metrics. |
|  | `SEC-005-control` | Mandatory logging of all failed API attempts. |

---

## 📚 Supersession Chain
- **Supersedes:** *None*  
- **Superseded by:** Future ADRs defining enterprise-wide audit schema and cross-domain telemetry unification.

---
