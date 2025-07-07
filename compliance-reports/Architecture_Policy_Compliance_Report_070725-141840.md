# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2025-07-07 14:18:39

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
- 1: The UML diagram indicates that the `MandateUpdateService` container has a relationship with the `ThoughtMachine` external system. This violates the policy rule which states that only the `VaultCoreClient` adapter is allowed to connect to the `ThoughtMachine` external system. The policy logic specifically checks for any source other than `VaultCoreClient` attempting to connect to `ThoughtMachine` and flags it as non-compliant.

Suggestions:
- 1: To fix this deviation, modify the architecture so that the `MandateUpdateService` does not directly connect to `ThoughtMachine`. Instead, ensure that all interactions with `ThoughtMachine` are routed through the `VaultCoreClient`. This can be achieved by having the `MandateUpdateService` call the `VaultCoreClient`, which then handles the connection to `ThoughtMachine`. Update the UML diagram to reflect this change in the relationships.

---

### Compliant Policies

#### ARC-POLICY-101 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Conclusion: The UML diagram complies with the provided OPA policy. All microservices (WebAPI, ServiceLayer, VaultCoreClient) are defined within a system boundary labeled 'ecs', which aligns with the policy requirement that containers must be deployed in ECS Fargate. Therefore, there are no deviations from the policy.

---

#### ARC-POLICY-102 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Conclusion: The UML diagram complies with the provided OPA policy. The relationships defined in the UML diagram ensure that the external system "ClientApp" connects directly to the "ApiGateway", which is consistent with the policy requirement that external systems must not access non-API-Gateway components directly. There are no deviations from the policy logic, and the expected policy output is an empty list, indicating compliance.

---

#### ARC-POLICY-104 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The `VaultCoreClient` container, which makes an external call to `ThoughtMachine`, is correctly annotated with the stereotype `<<uses_secret>>`, satisfying the policy requirement that containers making external calls must be marked as using secrets. Therefore, there are no deviations from the policy, and no suggestions for fixes are necessary.

---

#### ARC-POLICY-105 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram includes an "Audit Log" component, which is an Amazon RDS PostgreSQL data store. This satisfies the policy requirement for a data store related to audit or logging, as specified in the Rego policy. The policy logic checks for the presence of a container with a name containing "audit" or "log" and a technology related to PostgreSQL or RDS, or a stereotype of "data_store". The provided input JSON confirms the presence of such a container, and the expected policy output is an empty list, indicating compliance.

---

#### ARC-POLICY-106 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The policy requires that at least one container must have 'secrets' in the name or be labeled <<secrets_store>>. The UML diagram includes a container named "AWS Secrets Manager" with the stereotype <<secrets_store>>, which satisfies the policy requirement. Therefore, there are no deviations from the policy, and no suggestions for changes are necessary.

---

#### ARC-POLICY-108 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the policy ARC-POLICY-108. The policy requires that at least one container be labeled as <<observability>> or include 'cloudwatch' in the name. The UML diagram includes the "Amazon CloudWatch" container, which is labeled with the stereotype <<observability>>. Therefore, the architecture meets the observability component requirement specified in the policy.

---

