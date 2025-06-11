# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2025-06-11 11:30:00

## Summary of Compliance

| Policy ID      | Compliance Status |
|----------------|-------------------|
| ARC-POLICY-101      | ✅ Yes    |
| ARC-POLICY-102      | ❌ No    |
| ARC-POLICY-103      | ❌ No    |
| ARC-POLICY-104      | ✅ Yes    |
| ARC-POLICY-105      | ✅ Yes    |
| ARC-POLICY-106      | ✅ Yes    |
| ARC-POLICY-108      | ✅ Yes    |

## Detailed Policy Evaluations

### Non-Compliant Policies

#### ARC-POLICY-102 - ❌ NON-COMPLIANT

**Evaluation Details:**
Compliance: No

Deviations:
- 1: The UML diagram shows a relationship from `ThoughtMachine` (an external system) directly to `AdapterClient`, which is not an API Gateway. This violates the policy that external systems must not connect directly to non-API-Gateway components.

Suggestions:
- 1: Introduce an API Gateway component between `ThoughtMachine` and `AdapterClient`. Update the UML diagram to show `ThoughtMachine` connecting to this new API Gateway, which then forwards requests to `AdapterClient`. This ensures compliance by routing external system interactions through an API Gateway.

---

#### ARC-POLICY-103 - ❌ NON-COMPLIANT

**Evaluation Details:**
Compliance: No

Deviations:
- 1: The UML diagram shows a relationship where the "MandateUpdateService" container connects directly to the "ThoughtMachine" external system. This violates the policy which explicitly states that only the "VaultCoreClient" adapter is allowed to connect to the "ThoughtMachine" external system.

Suggestions:
- 1: Modify the architecture so that the "MandateUpdateService" does not directly connect to "ThoughtMachine". Instead, ensure that all interactions with "ThoughtMachine" are routed through the "VaultCoreClient". This can be achieved by having the "MandateUpdateService" call the "VaultCoreClient" for any operations that require access to "ThoughtMachine". Adjust the UML diagram to reflect this change in the relationship.

---

### Compliant Policies

#### ARC-POLICY-101 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Conclusion: The UML diagram complies with the provided OPA policy. All microservices (WebAPI, ServiceLayer, VaultCoreClient) are defined within a system boundary labeled "ecs," which aligns with the policy requirement for deployment in ECS Fargate.

---

#### ARC-POLICY-104 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The policy checks if any container making an external call is not marked as using secrets. In the provided input JSON, the `VaultCoreClient` container, which makes an external call to `ThoughtMachine`, is correctly annotated with the stereotype `uses_secret`. Therefore, there are no deviations from the policy, and the architecture is compliant.

---

#### ARC-POLICY-105 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The policy requires the architecture to include an audit log or persistent data store related to audit or logging, specifically using PostgreSQL or RDS technology, or having a stereotype of "data_store". The UML diagram includes a container named "Audit Log" with the technology "Amazon RDS PostgreSQL" and the stereotype "data_store", which satisfies the policy requirements. Therefore, there are no deviations from the policy, and no suggestions for fixes are necessary.

---

#### ARC-POLICY-106 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Conclusion: The UML diagram complies with the policy ARC-POLICY-106. The architecture includes a component named "AWS Secrets Manager" with the alias "Secrets," which is labeled with the stereotype "secrets_store." This satisfies the policy requirement that at least one container must have 'secrets' in the name or be labeled <<secrets_store>>.

---

#### ARC-POLICY-108 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the policy ARC-POLICY-108. The policy requires at least one container to be labeled as <<observability>> or include 'cloudwatch' in the name. The diagram includes the "Amazon CloudWatch" container, which is labeled with the stereotype <<observability>>, satisfying the policy requirement.

---

