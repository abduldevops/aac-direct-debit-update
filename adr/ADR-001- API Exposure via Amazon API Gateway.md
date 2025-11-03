---
id: ADR-001
title: API Exposure via Amazon API Gateway
status: Accepted
date: 2025-11-03
context: Direct Debit Mandate Update Microservice
linkedPolicies:
  - paC/api-authentication.rego
  - paC/api-versioning.rego
  - paC/api-security.rego
linkedControls:
  - INFRA-002-control
  - SEC-001-control
  - SEC-004-control
  - ARC-006-control
tags: [api, security, aws, gateway, architecture, directdebit]
supersedes: null
---

## 🧩 Context
The Direct Debit Mandate Update microservice exposes APIs to the CRM and Client Portal for mandate creation and modification.  
Given the sensitive financial nature of these operations and regulatory requirements for authentication, authorization, and auditability, API ingress must be consistent, secure, and observable.

Historically, services within the financial ecosystem have exposed REST endpoints directly through the application layer. This created inconsistencies in authentication handling, increased exposure to DDoS risks, and limited centralized monitoring.

To align with the **Architecture as Code** governance model and AWS cloud-native best practices, the service must adopt a consistent ingress standard using **Amazon API Gateway** with HTTPS and OAuth2 authentication enforcement.

---

## 💡 Decision
All external-facing REST APIs (e.g., `/api/v1/directdebit/create`, `/api/v1/directdebit/update`) must be exposed via **Amazon API Gateway**.  
API Gateway configurations must:
- Use **HTTPS-only** endpoints.  
- Enforce **OAuth2 authentication** and token validation before requests reach backend services.  
- Support **versioned paths** (`/api/v1/...`) to manage future API evolution.  
- Emit metrics and access logs to **Amazon CloudWatch** for observability.  
- Apply **rate limiting and throttling policies** to prevent misuse and ensure service resilience.

The PlantUML architecture diagrams must include an `<<api_gateway>>` container, showing the interaction boundary between the CRM/Portal and the Direct Debit Mandate Update microservice.

---

## ⚙️ Implementation & Enforcement
### Enforcement Mechanisms
- **Policies-as-Code (PaC):**
  - `paC/api-authentication.rego` – Enforces OAuth2 and signed request validation.
  - `paC/api-versioning.rego` – Ensures versioned API path structure.
  - `paC/api-security.rego` – Blocks deployment of non-HTTPS endpoints.

- **Controls-as-Code (CaC):**
  - `INFRA-002-control` – API Gateway must use HTTPS-only endpoints.
  - `SEC-001-control` – All REST endpoints must require authentication.
  - `SEC-004-control` – OAuth2 clients must not bypass token validation.
  - `ARC-006-control` – Controllers must be versioned under `/api/v1/...`.

These controls are validated via GitHub Actions CI and OPA (Open Policy Agent) pipeline checks. Any pull request violating these rules will fail pre-merge validation.

---

## 📈 Consequences
**Positive:**
- Centralized enforcement of security, throttling, and monitoring.
- Reduced attack surface by removing direct backend exposure.
- Improved auditability and compliance with FCA, GDPR, and internal InfoSec requirements.
- Clear architectural consistency across all services under the Fargate runtime.

**Negative / Trade-offs:**
- Slight increase in latency (sub-millisecond) due to Gateway routing.
- Additional cost from API Gateway and CloudWatch usage.
- Requires ongoing maintenance of OAuth2 configuration and client secrets in AWS Secrets Manager.

---

## 🧭 Status & Next Steps
- This ADR applies to all future Direct Debit-related microservices.
- Review alignment quarterly as part of the Architecture Review Board (ARB) process.
- Future ADRs (e.g., ADR-008 on **Throttling & Quota Strategy**) may supersede or extend this decision.

---

## 🔗 Traceability
| Type | Reference | Description |
|------|------------|-------------|
| **Policy** | `paC/api-authentication.rego` | Enforces OAuth2 authentication |
|  | `paC/api-versioning.rego` | Validates versioned endpoint structure |
|  | `paC/api-security.rego` | Ensures HTTPS and signed request validation |
| **Control** | `INFRA-002-control` | HTTPS-only enforcement on API Gateway |
|  | `SEC-001-control` | All endpoints require authentication |
|  | `SEC-004-control` | OAuth2 token validation required |
|  | `ARC-006-control` | Enforces controller versioning structure |

---

## 📚 Supersession Chain
- **Supersedes:** *None*  
- **Superseded by:** Future ADRs extending gateway configuration, throttling, or multi-region routing policies.

---
