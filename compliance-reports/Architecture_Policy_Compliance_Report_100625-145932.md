# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2025-06-10 14:59:31

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
- 1: The UML diagram shows a relationship where the "MandateUpdateService" container connects directly to the "ThoughtMachine" external system. This violates the policy, which explicitly states that only the "VaultCoreClient" adapter is allowed to connect to the "ThoughtMachine" external system.

Suggestions:
- 1: Modify the architecture so that the "MandateUpdateService" does not directly connect to "ThoughtMachine". Instead, ensure that all interactions with "ThoughtMachine" are routed through the "VaultCoreClient" adapter. This could involve refactoring the "MandateUpdateService" to call the "VaultCoreClient" for any operations that require access to "ThoughtMachine".

---

### Compliant Policies

#### ARC-POLICY-101 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Conclusion:
The UML diagram complies with the provided OPA policy. All microservices (WebAPI, ServiceLayer, VaultCoreClient) are defined within a system boundary labeled "ecs," which satisfies the policy requirement that containers must be deployed in ECS Fargate.

---

#### ARC-POLICY-102 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram adheres to the provided OPA policy. The relationships specified in the policy input JSON match the connections in the UML diagram, and there are no direct connections from external systems to non-API-Gateway components. The external system "ClientApp" correctly connects to "ApiGateway," which is an approved API Gateway alias according to the policy.

---

#### ARC-POLICY-104 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The policy checks for containers that make external calls and ensures they are marked with the stereotype "uses_secret". The UML diagram includes the `VaultCoreClient` container, which makes an external call to `ThoughtMachine` and is correctly annotated with the stereotype "uses_secret". Therefore, there are no deviations from the policy, and the architecture is compliant.

---

#### ARC-POLICY-105 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

Conclusion: The UML diagram complies with the policy ARC-POLICY-105. The architecture includes an audit log data store named "Audit Log" using "Amazon RDS PostgreSQL," which satisfies the requirement for a persistent data store related to audit or logging as specified in the policy.

---

#### ARC-POLICY-106 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the policy ARC-POLICY-106. The policy requires that at least one container must have 'secrets' in the name or be labeled <<secrets_store>>. The UML diagram includes a container named "AWS Secrets Manager" with the stereotype <<secrets_store>>, fulfilling the policy requirement. Therefore, there are no deviations from the policy, and no suggestions for changes are necessary.

---

#### ARC-POLICY-108 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the policy ARC-POLICY-108. The policy requires at least one container to be labeled as <<observability>> or include 'cloudwatch' in the name. The UML diagram includes the "Amazon CloudWatch" container, which is labeled with the stereotype <<observability>>, satisfying the policy requirement. Therefore, there are no deviations from the policy, and no suggestions for changes are necessary.

---

