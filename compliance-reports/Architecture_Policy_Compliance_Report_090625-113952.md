# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2025-06-09 11:39:51

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
- 1: The UML diagram shows a relationship from `ThoughtMachine` (an external system) to `AdapterClient`, which is not routed through an `ApiGateway`. This violates the policy that requires all external systems to connect only through an API Gateway.

Suggestions:
- 1: Introduce an `ApiGateway` component between `ThoughtMachine` and `AdapterClient`. This would ensure that the external system (`ThoughtMachine`) communicates with internal services only through the designated API Gateway, complying with the policy.

By addressing the above deviation, the architecture will align with the policy requirements.

---

#### ARC-POLICY-103 - ❌ NON-COMPLIANT

**Evaluation Details:**
Compliance: No

Deviations:
- 1: The UML diagram shows a relationship where the "MandateUpdateService" container connects directly to the "ThoughtMachine" external system. This violates the policy rule that only the "VaultCoreClient" adapter is allowed to connect to the "ThoughtMachine" external system. The policy logic explicitly checks that any connection to "ThoughtMachine" must originate from "VaultCoreClient", and any other source is denied.

Suggestions:
- 1: Modify the architecture so that the "MandateUpdateService" does not directly connect to "ThoughtMachine". Instead, ensure that all interactions with "ThoughtMachine" are routed through the "VaultCoreClient". This can be achieved by having "MandateUpdateService" call "VaultCoreClient" for any operations that require access to "ThoughtMachine". This change will ensure compliance with the policy by centralizing the external system access through the designated adapter.

---

### Compliant Policies

#### ARC-POLICY-101 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Conclusion: The UML diagram complies with the provided OPA policy. All microservices (WebAPI, ServiceLayer, VaultCoreClient) are defined within a system boundary labeled "ECS," which meets the policy requirement that containers must be deployed in ECS Fargate.

---

#### ARC-POLICY-104 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The `VaultCoreClient` container, which makes an external call to `ThoughtMachine`, is correctly annotated with the stereotype `<<uses_secret>>`, fulfilling the policy requirement that containers making external calls must be marked as using secrets.

---

#### ARC-POLICY-105 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram includes an "Audit Log" component, which is an Amazon RDS PostgreSQL data store. This satisfies the policy requirement for a data store related to audit or logging, as it contains the necessary attributes: the name includes "audit," the technology is "Amazon RDS PostgreSQL," and it is marked with the stereotype "data_store." Therefore, the architecture complies with the policy ARC-POLICY-105.

---

#### ARC-POLICY-106 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the policy ARC-POLICY-106. The policy requires that at least one container must have 'secrets' in the name or be labeled <<secrets_store>>. The UML diagram includes a container named "AWS Secrets Manager" with the stereotype <<secrets_store>>, satisfying the policy requirement. Therefore, there are no deviations from the policy, and no suggestions for changes are necessary.

---

#### ARC-POLICY-108 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the policy ARC-POLICY-108. The policy requires at least one container to be labeled as <<observability>> or include 'cloudwatch' in the name. The diagram includes a container named "Amazon CloudWatch" with the stereotype <<observability>>, fulfilling the policy requirement. Therefore, there are no deviations, and no suggestions for changes are necessary.

---

