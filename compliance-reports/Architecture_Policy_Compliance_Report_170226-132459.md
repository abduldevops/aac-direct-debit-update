# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2026-02-17 13:24:57

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
- **1:** The UML diagram indicates that the `MandateUpdateService` directly connects to the `ThoughtMachine` external system. This violates the policy rule, which explicitly states that only the `VaultCoreClient` adapter is allowed to connect to the `ThoughtMachine` external system. The policy logic ensures that external system access is restricted to a designated adapter for security and compliance purposes.

**Suggestions:**
- **1:** Modify the UML diagram to ensure that the `MandateUpdateService` does not directly connect to `ThoughtMachine`. Instead, route all interactions with `ThoughtMachine` through the `VaultCoreClient` adapter. Update the relationship in the UML diagram to reflect this change:
  - Remove the relationship: `Rel(ServiceLayer, ThoughtMachine, "Calls Vault Core API")`
  - Add a new relationship: `Rel(ServiceLayer, AdapterClient, "Delegates Vault Core API calls")`

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
The UML diagram defines all microservices (`WebAPI`, `ServiceLayer`, and `VaultCoreClient`) within the `ECS Fargate Cluster` system boundary labeled `ecs`. This adheres to the policy requirement that microservices must be deployed within a system boundary prefixed with `ecs`. The provided policy input JSON confirms that all containers are correctly associated with the `ecs` boundary. Therefore, the architecture complies fully with the policy ARC-POLICY-101.

---

#### ARC-POLICY-102 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram adheres to the provided OPA policy (ARC-POLICY-102). The policy explicitly states that external systems (defined as `ClientApp`, `ExternalUser`, or `CRM`) must not connect directly to non-API-Gateway components. In the UML diagram, the external system `ClientApp` connects only to `ApiGateway`, which is a valid API Gateway alias as per the policy. All subsequent connections from `ApiGateway` to internal components (`WebAPI`, `ServiceLayer`, etc.) are compliant with the policy logic. Therefore, no violations are present.

---

#### ARC-POLICY-104 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:  
- None  

Suggestions:  
- None  

Explanation:  
The UML diagram complies with the provided OPA policy (ARC-POLICY-104). The `VaultCoreClient` container, which makes external calls to the `ThoughtMachine` system, is correctly annotated with the stereotype `<<uses_secret>>`. This satisfies the policy requirement that any container making external calls must be marked as using secrets. Additionally, the policy input JSON confirms that the `VaultCoreClient` container is appropriately configured, and the expected policy output JSON is empty, indicating no violations.

---

#### ARC-POLICY-105 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:  
- None  

Suggestions:  
- None  

Explanation:  
The UML diagram includes an audit log data store (`AuditStore`) that uses `Amazon RDS PostgreSQL` as its technology and is explicitly marked with the stereotype `data_store`. This satisfies the policy requirement that the architecture must include a persistent data store for audit or logging purposes. The provided policy input JSON also confirms the presence of the `AuditStore` container, and its attributes align with the policy logic. Therefore, the architecture complies fully with the policy ARC-POLICY-105.

---

#### ARC-POLICY-106 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram adheres to the policy ARC-POLICY-106. The policy requires at least one container to have "secrets" in its name or be labeled with the stereotype `<<secrets_store>>`. The UML diagram includes the `AWS Secrets Manager` container, which satisfies the policy requirements as it is explicitly named "Secrets" and has the stereotype `<<secrets_store>>`. Therefore, no deviations are present, and the architecture is compliant.

---

#### ARC-POLICY-108 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Explanation:
The UML diagram adheres to the provided OPA policy (ARC-POLICY-108). The policy requires at least one container to be labeled as `<<observability>>` or include "cloudwatch" in its name. The `Monitoring` container, representing "Amazon CloudWatch," satisfies this requirement as it is explicitly labeled with the stereotype `<<observability>>`. Therefore, the architecture complies with the policy, and no deviations are present.

---

