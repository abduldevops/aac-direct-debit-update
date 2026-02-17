# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2026-02-17 11:53:33

## Summary of Compliance

| Policy ID      | Compliance Status |
|----------------|-------------------|
| ARC-POLICY-101      | ✅ Yes    |
| ARC-POLICY-102      | ✅ Yes    |
| ARC-POLICY-103      | ❌ No    |
| ARC-POLICY-104      | ✅ Yes    |
| ARC-POLICY-105      | ✅ Yes    |
| ARC-POLICY-106      | ✅ Yes    |
| ARC-POLICY-108      | ✅ Yes    |

## Detailed Policy Evaluations

### Non-Compliant Policies

#### ARC-POLICY-103 - ❌ NON-COMPLIANT

**Evaluation Details:**
**Compliance:** No

**Deviations:**
- **1:** The UML diagram indicates that the `MandateUpdateService` directly connects to the `ThoughtMachine` external system. This violates the policy rule, which explicitly states that only the `VaultCoreClient` adapter is allowed to connect to `ThoughtMachine`. The policy logic ensures that external system access is restricted to a designated adapter for secure and controlled integration.

**Suggestions:**
- **1:** Modify the UML diagram to ensure that the `MandateUpdateService` does not directly connect to `ThoughtMachine`. Instead, route all interactions with `ThoughtMachine` through the `VaultCoreClient` adapter. Update the relationship in the UML diagram to reflect this change:
  - Remove the relationship: `Rel(ServiceLayer, ThoughtMachine, "Calls Vault Core API")`.
  - Add a new relationship: `Rel(ServiceLayer, AdapterClient, "Delegates Vault Core API calls")`.

**Conclusion:** No

---

### Compliant Policies

#### ARC-POLICY-101 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram adheres to the provided OPA policy. All microservices (`WebAPI`, `ServiceLayer`, and `VaultCoreClient`) are defined within the system boundary labeled `ecs`, which corresponds to the ECS Fargate cluster. This satisfies the policy requirement that containers must be deployed in ECS Fargate boundaries. No violations were detected.

---

#### ARC-POLICY-102 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram adheres to the provided OPA policy (ARC-POLICY-102). The policy explicitly checks that external systems (defined as `ClientApp`, `ExternalUser`, and `CRM`) do not directly connect to any component other than an API Gateway (`ApiGateway`). 

In the UML diagram:
1. The external system `ClientApp` connects directly to `ApiGateway`, which is compliant with the policy.
2. All subsequent connections from `ApiGateway` (to `WebAPI`, and further downstream components) are internal and do not involve direct access by external systems.

Thus, there are no violations of the policy, and the architecture is compliant.

---

#### ARC-POLICY-104 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram adheres to the provided OPA policy (ARC-POLICY-104). The `VaultCoreClient` container, which makes external calls to the `ThoughtMachine` system, is correctly annotated with the stereotype `<<uses_secret>>`. This satisfies the policy requirement that any container making external calls must be marked as using secrets. Additionally, the input JSON confirms that the `VaultCoreClient` container is appropriately configured, and the expected policy output is an empty array (`[]`), indicating no violations.

---

#### ARC-POLICY-105 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram includes a container named `Audit Log` with the alias `AuditStore`, which is explicitly defined as a persistent data store using `Amazon RDS PostgreSQL`. This satisfies the policy requirement that the architecture must include an audit log or persistent data store related to audit or logging. The policy input JSON also confirms the presence of this container with the correct attributes (`technology: Amazon RDS PostgreSQL`, `stereotype: data_store`). Therefore, the architecture complies fully with the policy ARC-POLICY-105.

---

#### ARC-POLICY-106 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram includes a container named "AWS Secrets Manager" with the alias "Secrets" and is labeled with the stereotype `secrets_store`. This satisfies the policy requirement that at least one container must either have "secrets" in its name or be labeled `<<secrets_store>>`. Therefore, the architecture complies fully with the provided policy ARC-POLICY-106.

---

#### ARC-POLICY-108 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram includes the `Amazon CloudWatch` container, which is labeled with the stereotype `observability` and explicitly named "CloudWatch." This satisfies the policy requirement that at least one container must be labeled as `<<observability>>` or include "cloudwatch" in its name. Therefore, the architecture adheres to the policy ARC-POLICY-108.

---

