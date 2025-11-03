---
id: ADR-002
title: Secrets and Credentials Management
status: Accepted
date: 2025-11-03
context: Direct Debit Mandate Update Microservice
linkedPolicies:
  - ARC-POLICY-104-control
  - ARC-POLICY-105-control
  - ARC-POLICY-106-control
linkedControls:
  - SEC-003-control
  - INFRA-004-control
tags: [security, secrets, aws, architecture, compliance, directdebit]
supersedes: null
---

## 🔐 Context
The Direct Debit Mandate Update microservice integrates with multiple external systems, including **Thought Machine Vault Core** and **Amazon RDS**.  
Each integration requires secure handling of API keys, OAuth2 credentials, and database connection parameters.  

Previous approaches in legacy systems relied on configuration files or environment variables containing plaintext secrets, leading to:
- **Security vulnerabilities** (credential leaks in logs or repos).  
- **Inconsistent secret rotation** across environments.  
- **Auditability gaps** in tracing credential sources.

To meet the organization’s cloud-native governance and compliance standards, **all secrets must be managed and injected securely** via **AWS Secrets Manager** with traceable architecture annotations and runtime validation.

---

## 💡 Decision
All services and adapter components that access external systems or data stores must:

1. **Declare a `<<uses_secret>>` annotation** in PlantUML diagrams.  
2. **Source credentials exclusively from AWS Secrets Manager** using IAM-based access control.  
3. **Prohibit hardcoded credentials** in source code, YAML files, or configuration templates.  
4. **Declare an explicit `Secrets Manager` dependency** in the architecture diagram (`<<secrets_manager>>` container).  
5. **Enforce secret access auditing and version rotation** in accordance with platform policy.  

Runtime access to secrets must occur through **Spring Cloud AWS** or the approved AWS SDK to ensure secure retrieval with minimal exposure time.

---

## ⚙️ Implementation & Enforcement

### Enforcement Mechanisms

- **Policies-as-Code (PaC):**
  - `ARC-POLICY-104-control` – Containers making external calls must be annotated `<<uses_secret>>`.  
  - `ARC-POLICY-105-control` – No containers may hardcode credentials.  
  - `ARC-POLICY-106-control` – A `Secrets Manager` dependency must be declared.  

- **Controls-as-Code (CaC):**
  - `SEC-003-control` – Secrets must not be hardcoded in code or config.  
  - `INFRA-004-control` – Secrets Manager must store DB and API credentials.  

**Validation Methods**
- **Static analysis** using ArchUnit and custom regex scans to detect credential literals.  
- **OPA/Rego policy checks** in GitHub Actions to verify `<<uses_secret>>` annotations in `.puml` diagrams.  
- **Terraform validation (tfsec)** to ensure all secret references point to `aws_secretsmanager_secret`.  
- **Runtime test harnesses** verifying credential retrieval from Secrets Manager, not environment variables.

---

## 📈 Consequences
**Positive:**
- Eliminates plaintext secrets in source repositories.  
- Centralizes management and rotation of credentials in AWS Secrets Manager.  
- Improves audit readiness and compliance with **FCA**, **GDPR**, and **ISO 27001**.  
- Enables traceable mapping between architecture, policies, and runtime secrets.  

**Negative / Trade-offs:**
- Slight overhead in initial IAM configuration and pipeline integration.  
- Increased reliance on AWS Secrets Manager availability.  
- Secrets must be granted least-privilege access through IAM roles, requiring coordination between DevOps and Architecture teams.  

---

## 🧭 Status & Next Steps
- Applied immediately to all **Direct Debit**-related microservices.  
- Future ADRs (e.g., ADR-008 on *Secret Rotation Automation*) may extend this decision.  
- Review and verify enforcement quarterly during Architecture Assurance reviews.  

---

## 🔗 Traceability
| Type | Reference | Description |
|------|------------|-------------|
| **Policy** | `ARC-POLICY-104-control` | Containers that call external systems must use `<<uses_secret>>`. |
|  | `ARC-POLICY-105-control` | Prohibits hardcoded credentials. |
|  | `ARC-POLICY-106-control` | Requires declared dependency on Secrets Manager. |
| **Control** | `SEC-003-control` | Secrets must not be hardcoded in code or config. |
|  | `INFRA-004-control` | Secrets Manager must store DB and API credentials. |

---

## 📚 Supersession Chain
- **Supersedes:** *None*  
- **Superseded by:** Future ADRs addressing automated secret rotation, audit integration, or cross-account credential federation.

---
