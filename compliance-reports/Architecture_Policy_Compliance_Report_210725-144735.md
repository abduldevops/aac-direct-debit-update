# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2025-07-21 14:47:35

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
Compliance: No

Deviations:
- 1: The UML diagram indicates that the "MandateUpdateService" container has a relationship with the "ThoughtMachine" external system. This violates the policy which states that only the "VaultCoreClient" adapter is allowed to connect to the "ThoughtMachine" external system. The policy logic explicitly checks for any source other than "VaultCoreClient" attempting to connect to "ThoughtMachine" and flags it as non-compliant.

Suggestions:
- 1: To fix this deviation, ensure that the "MandateUpdateService" does not directly connect to the "ThoughtMachine" external system. Instead, route all interactions with "ThoughtMachine" through the "VaultCoreClient" adapter. This can be achieved by modifying the "MandateUpdateService" to call the "VaultCoreClient" for any operations that require interaction with "ThoughtMachine". This change will align the architecture with the policy requirements by ensuring that only the designated adapter handles external communications with "ThoughtMachine".

---

### Compliant Policies

#### ARC-POLICY-101 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. All microservices (WebAPI, ServiceLayer, and VaultCoreClient) are defined within a system boundary labeled 'ecs', which aligns with the policy requirement that containers must be deployed in ECS Fargate. Therefore, there are no deviations from the policy, and no suggestions for changes are necessary.

---

#### ARC-POLICY-102 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram adheres to the provided OPA policy. The policy specifies that external systems should not connect directly to non-API-Gateway components. In the UML diagram, the external system "ClientApp" connects to "ApiGateway," which is a valid API Gateway alias according to the policy. There are no direct connections from external systems to non-API-Gateway components, ensuring compliance with the policy.

---

#### ARC-POLICY-104 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The `VaultCoreClient` container, which makes an external call to the `ThoughtMachine` system, is correctly annotated with the stereotype `<<uses_secret>>`, satisfying the policy requirement. There are no deviations from the policy logic, and thus, no actionable fixes are necessary.

---

#### ARC-POLICY-105 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram includes an "Audit Log" data store using "Amazon RDS PostgreSQL," which satisfies the policy requirement for a data store related to audit or logging. The policy checks for the presence of a container with "audit" or "log" in its name and ensures it uses a technology like "PostgreSQL" or "RDS," which is fulfilled by the "AuditStore" container in the provided input. Therefore, the architecture complies with the policy ARC-POLICY-105.

---

#### ARC-POLICY-106 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The policy requires that at least one container must have 'secrets' in the name or be labeled with the stereotype <<secrets_store>>. The UML diagram includes a container named "AWS Secrets Manager" with the stereotype <<secrets_store>>, satisfying the policy requirement. Therefore, there are no deviations, and the architecture is compliant.

---

#### ARC-POLICY-108 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The policy requires at least one container to be labeled as <<observability>> or include 'cloudwatch' in the name. The "Amazon CloudWatch" container is labeled with the stereotype "observability," satisfying the policy requirement. Therefore, there are no deviations, and the architecture is compliant.

---

