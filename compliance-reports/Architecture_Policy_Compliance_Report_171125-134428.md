# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2025-11-17 13:44:27

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
- 1: The UML diagram indicates that the `MandateUpdateService` container has a relationship with the `ThoughtMachine` external system. This violates the policy, which explicitly states that only the `VaultCoreClient` adapter is allowed to connect to the `ThoughtMachine` external system. The policy logic checks for any source other than `VaultCoreClient` attempting to connect to `ThoughtMachine` and flags it as non-compliant.

Suggestions:
- 1: Modify the architecture so that the `MandateUpdateService` does not directly connect to the `ThoughtMachine` external system. Instead, ensure that all interactions with `ThoughtMachine` are routed through the `VaultCoreClient`. This can be achieved by having the `MandateUpdateService` call the `VaultCoreClient` for any operations that require interaction with `ThoughtMachine`, thereby adhering to the policy requirement.

---

### Compliant Policies

#### ARC-POLICY-101 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. All microservices (WebAPI, ServiceLayer, and VaultCoreClient) are defined within a system boundary labeled "ecs," which aligns with the policy requirement that containers must be deployed in an ECS Fargate cluster. Therefore, there are no deviations from the policy.

---

#### ARC-POLICY-102 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram adheres to the provided OPA policy. The policy specifies that external systems must not connect directly to non-API-Gateway components. In the UML diagram, the external system "ClientApp" connects to "ApiGateway," which is an allowed API Gateway alias. There are no direct connections from external systems to non-API-Gateway components, thus complying with the policy.

---

#### ARC-POLICY-104 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The `VaultCoreClient` container, which makes an external call to the `ThoughtMachine` system, is correctly annotated with the `<<uses_secret>>` stereotype, indicating it uses secrets. This satisfies the policy requirement that any container making external calls must be marked as using secrets. Therefore, there are no deviations from the policy, and the architecture is compliant.

---

#### ARC-POLICY-105 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram includes an "Audit Log" component, which is an Amazon RDS PostgreSQL data store. This satisfies the policy requirement for having a data store related to audit or logging, as specified by the policy logic. The policy checks for the presence of a data store with "audit" or "log" in its name and that uses PostgreSQL or RDS technology, which is exactly what the "Audit Log" component provides. Therefore, the architecture complies with the policy.

---

#### ARC-POLICY-106 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram and the provided policy input JSON comply with the policy ARC-POLICY-106. The architecture includes a container named "AWS Secrets Manager" with the alias "Secrets," which is labeled with the stereotype "secrets_store." This satisfies the policy requirement that at least one container must have 'secrets' in the name or be labeled <<secrets_store>>. Therefore, there are no deviations from the policy, and the architecture is compliant.

---

#### ARC-POLICY-108 - ✅ COMPLIANT

**Evaluation Details:**
Compliance: Yes

Deviations:
- None

Suggestions:
- None

The UML diagram complies with the provided OPA policy. The policy requires at least one container to be labeled as <<observability>> or include 'cloudwatch' in the name. The input JSON indicates that the "Amazon CloudWatch" container is labeled with the stereotype "observability", satisfying the policy requirement. Therefore, there are no deviations, and the architecture is compliant.

---

