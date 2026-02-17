# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2026-02-17 13:50:04

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
- **1:** The `MandateUpdateService` container is directly accessing the `ThoughtMachine` external system.  
  **Reason:** According to the provided OPA policy (`ARC-POLICY-103`), only the `VaultCoreClient` adapter is allowed to connect to the `ThoughtMachine` external system. The policy explicitly denies any other container from accessing `ThoughtMachine`. The UML diagram shows a relationship where `MandateUpdateService` calls `ThoughtMachine`, which violates this rule.

**Suggestions:**
- **1:** Update the architecture so that `MandateUpdateService` does not directly interact with `ThoughtMachine`. Instead, route all interactions with `ThoughtMachine` through the `VaultCoreClient` adapter. Specifically:
  - Remove the direct relationship between `MandateUpdateService` and `ThoughtMachine`.
  - Ensure `MandateUpdateService` delegates external API calls to `VaultCoreClient`, which will handle communication with `ThoughtMachine`.

**Conclusion:** The UML diagram does not comply with the policy.

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
The UML diagram adheres to the provided OPA policy (`ARC-POLICY-101`). All microservices (`WebAPI`, `ServiceLayer`, and `VaultCoreClient`) are defined within the system boundary labeled `ecs`, which corresponds to the ECS Fargate cluster. This satisfies the policy requirement that containers must be deployed within an ECS Fargate boundary. No violations were identified.

---

#### ARC-POLICY-102 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram adheres to the provided OPA policy (ARC-POLICY-102). The policy explicitly states that external systems (defined as `ClientApp`, `ExternalUser`, and `CRM`) must not directly connect to non-API Gateway components. The input JSON relationships confirm that the external system (`ClientApp`) only connects to `ApiGateway`, which is a valid API Gateway alias as per the policy. All subsequent connections (e.g., `ApiGateway` to `WebAPI`, `WebAPI` to `ServiceLayer`) occur within the internal architecture and do not involve direct access by external systems. Therefore, the architecture complies fully with the policy.

---

#### ARC-POLICY-104 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram adheres to the provided OPA policy (ARC-POLICY-104). The policy checks whether containers making external calls are annotated with the stereotype `<<uses_secret>>`. In the provided UML diagram, the container `VaultCoreClient` makes an external call to `ThoughtMachine` and is correctly annotated with `<<uses_secret>>`. The input JSON confirms this annotation, and there are no other containers making external calls that violate the policy. Therefore, the architecture complies with the policy.

---

#### ARC-POLICY-105 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram includes an audit log data store (`AuditStore`) with the technology `Amazon RDS PostgreSQL`, which satisfies the policy requirement for a persistent data store related to audit or logging. The policy explicitly checks for containers with names containing "audit" or "log" and technologies such as "PostgreSQL" or "RDS", and the `AuditStore` container meets these criteria. Therefore, the architecture complies with the policy ARC-POLICY-105.

---

#### ARC-POLICY-106 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram includes a container named "AWS Secrets Manager" with the alias "Secrets" and the stereotype `secrets_store`. This satisfies the policy requirement that at least one container must have "secrets" in its name or be labeled `<<secrets_store>>`. The provided input JSON also confirms the presence of this container, and the policy output matches the expected result (`[]`), indicating no violations.

---

#### ARC-POLICY-108 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram adheres to the provided policy (ARC-POLICY-108). The policy requires at least one container to be labeled as `<<observability>>` or include "cloudwatch" in its name. The input JSON confirms that the "Monitoring" container (Amazon CloudWatch) is labeled with the stereotype `<<observability>>`. Additionally, the UML diagram explicitly includes Amazon CloudWatch as an observability component, fulfilling the policy requirement. Therefore, there are no deviations, and the architecture complies with the policy.

---

