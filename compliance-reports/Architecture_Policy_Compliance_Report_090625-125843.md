# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2025-06-09 12:58:42

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
- 1: The UML diagram indicates that the `MandateUpdateService` container directly accesses the `ThoughtMachine` external system. This violates the policy rule which states that only the `VaultCoreClient` adapter is allowed to connect to the `ThoughtMachine` external system. The policy logic explicitly checks for any source other than `VaultCoreClient` attempting to connect to `ThoughtMachine` and flags it as non-compliant.

Suggestions:
- 1: Modify the architecture so that the `MandateUpdateService` does not directly connect to the `ThoughtMachine` external system. Instead, ensure that all interactions with `ThoughtMachine` are routed through the `VaultCoreClient`. This can be achieved by having the `MandateUpdateService` call the `VaultCoreClient`, which then handles the communication with `ThoughtMachine`. Update the UML diagram to reflect this change in the relationships section, ensuring compliance with the policy.

---

### Compliant Policies

#### ARC-POLICY-101 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Conclusion: The UML diagram complies with the provided OPA policy. All microservices (WebAPI, ServiceLayer, VaultCoreClient) are defined within a system boundary labeled 'ecs', which satisfies the policy requirement that containers must be deployed in ECS Fargate. Therefore, there are no deviations from the policy.

---

#### ARC-POLICY-102 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The relationships defined in the policy input JSON match those in the UML diagram, and no external system connects directly to a non-API-Gateway component. The policy checks for direct access violations from external systems to components other than the API Gateway, and no such violations are present in the diagram.

---

#### ARC-POLICY-104 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The container "VaultCoreClient" makes an external call to "ThoughtMachine" and is correctly annotated with the stereotype <<uses_secret>>, satisfying the policy requirement that containers making external calls must be marked as using secrets.

---

#### ARC-POLICY-105 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram includes a container named "Audit Log" with the technology "Amazon RDS PostgreSQL" and the stereotype "data_store." This satisfies the policy requirement for a data store related to audit or logging, as specified in the Rego policy logic. Therefore, the architecture complies with the policy ARC-POLICY-105.

---

#### ARC-POLICY-106 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the policy ARC-POLICY-106. The policy requires that at least one container must have 'secrets' in the name or be labeled <<secrets_store>>. The UML diagram includes a container named "AWS Secrets Manager" with the stereotype <<secrets_store>>, which satisfies the policy requirement. Therefore, there are no deviations from the policy, and no suggestions for changes are necessary.

---

#### ARC-POLICY-108 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the policy ARC-POLICY-108, as it includes the "Amazon CloudWatch" container, which is labeled with the stereotype "observability." This satisfies the policy requirement that at least one container must be labeled as <<observability>> or include 'cloudwatch' in the name. Therefore, there are no deviations from the policy, and no suggestions for changes are necessary.

---

