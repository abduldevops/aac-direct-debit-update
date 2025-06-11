# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2025-06-11 12:52:29

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
- 1: The UML diagram shows a relationship where the `MandateUpdateService` container connects directly to the `ThoughtMachine` external system. This violates the policy rule that only the `VaultCoreClient` adapter is allowed to connect to the `ThoughtMachine` external system. The policy logic explicitly checks that any connection to `ThoughtMachine` must originate from `VaultCoreClient`, and the presence of a connection from `MandateUpdateService` is against this rule.

Suggestions:
- 1: Modify the UML diagram to ensure that the `MandateUpdateService` does not directly connect to `ThoughtMachine`. Instead, route any necessary communication through the `VaultCoreClient`. This can be achieved by having `MandateUpdateService` communicate with `VaultCoreClient`, which then handles the interaction with `ThoughtMachine`. Adjust the relationships in the diagram accordingly to reflect this change.

---

### Compliant Policies

#### ARC-POLICY-101 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Conclusion: The UML diagram complies with the provided OPA policy. All microservices (WebAPI, ServiceLayer, VaultCoreClient) are correctly defined within a system boundary labeled "ecs," which aligns with the policy requirement for deployment in ECS Fargate.

---

#### ARC-POLICY-102 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram adheres to the provided OPA policy. The policy specifies that external systems should not connect directly to non-API-Gateway components. The relationships in the UML diagram show that the external system "ClientApp" connects to "ApiGateway," which is a valid API Gateway alias according to the policy. Therefore, there are no deviations from the policy, and the architecture is compliant.

---

#### ARC-POLICY-104 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The policy checks if any container making an external call is not marked as using secrets. In this case, the `VaultCoreClient` container makes an external call to `ThoughtMachine` and is correctly annotated with the stereotype `<<uses_secret>>`, satisfying the policy requirements. Therefore, there are no deviations from the policy.

---

#### ARC-POLICY-105 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram includes an "Audit Log" container with the technology "Amazon RDS PostgreSQL" and the stereotype "data_store". This satisfies the policy requirement for a data store related to audit or logging, as specified in the Rego policy. Therefore, the architecture complies with the policy ARC-POLICY-105.

---

#### ARC-POLICY-106 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram includes a component named "AWS Secrets Manager" with the stereotype "secrets_store," which satisfies the policy requirement that at least one container must have 'secrets' in the name or be labeled <<secrets_store>>. Therefore, the architecture complies with the policy ARC-POLICY-106.

---

#### ARC-POLICY-108 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the policy ARC-POLICY-108. The policy requires that at least one container must be labeled as <<observability>> or include 'cloudwatch' in the name. The provided input JSON indicates that the "Amazon CloudWatch" container is labeled with the stereotype "observability," which satisfies the policy requirement. Therefore, there are no deviations from the policy, and no suggestions for changes are necessary.

---

