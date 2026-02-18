# Architecture Policy Evaluation Report

**Diagram**: `DirectDebitMandateUpdateArchitectureComponentDiagram.puml`
**Date**: 2026-02-18 14:14:32

## Summary of Compliance

| Policy ID      | Compliance Status |
|----------------|-------------------|
| ARC-POLICY-101      | ❌ No    |
| ARC-POLICY-102      | ❌ No    |
| ARC-POLICY-103      | ❌ No    |
| ARC-POLICY-104      | ❌ No    |
| ARC-POLICY-105      | ❌ No    |
| ARC-POLICY-106      | ❌ No    |
| ARC-POLICY-108      | ❌ No    |

## Detailed Policy Evaluations

### Non-Compliant Policies

#### ARC-POLICY-101 - ❌ NON-COMPLIANT

**Evaluation Details:**
**Compliance:** Yes

**Deviations:**  
- None  

**Suggestions:**  
- None  

**Explanation:**  
The UML diagram adheres to the provided OPA policy (`ARC-POLICY-101`). The policy mandates that all microservices must be deployed within a system boundary labeled "Fargate" or "ECS." Based on the input JSON and the UML diagram:  
1. All containers (`WebAPI`, `ServiceLayer`, and `VaultCoreClient`) are defined within the `ecs` boundary, which satisfies the policy condition (`startswith(container.boundary, "ecs")`).  
2. There are no containers outside the `ecs` boundary, and no violations of the policy logic are observed.  

Thus, the architecture is fully compliant with the policy.

---

#### ARC-POLICY-102 - ❌ NON-COMPLIANT

**Evaluation Details:**
**Compliance:** Yes

**Deviations:**  
- None  

**Suggestions:**  
- None  

**Explanation:**  
The UML diagram adheres to the provided OPA policy (ARC-POLICY-102). The policy explicitly states that external systems (e.g., `ClientApp`) must not directly connect to any component other than an API Gateway. Based on the relationships in the UML diagram and the input JSON, the `ClientApp` connects to `ApiGateway`, which is a valid API Gateway alias as defined in the policy. All other connections are internal and do not involve external systems violating the policy.  

Thus, the architecture is compliant with the policy.

---

#### ARC-POLICY-103 - ❌ NON-COMPLIANT

**Evaluation Details:**
**Compliance:** No

**Deviations:**
- **1:** The `MandateUpdateService` container is directly connecting to the `ThoughtMachine` external system.  
  - **Reason:** According to the policy (`ARC-POLICY-103`), only the `VaultCoreClient` adapter is allowed to connect to the `ThoughtMachine` external system. This ensures a single, controlled integration point for external system communication. The direct connection from `MandateUpdateService` violates this rule.

**Suggestions:**
- **1:** Refactor the architecture to ensure that the `MandateUpdateService` container does not directly connect to the `ThoughtMachine` external system. Instead, route all communication to `ThoughtMachine` through the `VaultCoreClient` adapter. Update the UML diagram to reflect this change by removing the relationship between `MandateUpdateService` and `ThoughtMachine`.

**Conclusion:** The UML diagram does not comply with the policy.

---

#### ARC-POLICY-104 - ❌ NON-COMPLIANT

**Evaluation Details:**
**Compliance:** Yes

**Deviations:**  
- None  

**Suggestions:**  
- None  

**Explanation:**  
The UML diagram adheres to the provided OPA policy (ARC-POLICY-104). The policy checks whether any container making external calls is not annotated with the `<<uses_secret>>` stereotype. In this case:
1. The `VaultCoreClient` container is the only container making an external call (to `ThoughtMachine`).
2. The `VaultCoreClient` container is correctly annotated with the `<<uses_secret>>` stereotype, as confirmed in the Policy Input JSON.

Thus, there are no violations of the policy, and the architecture is compliant.

---

#### ARC-POLICY-105 - ❌ NON-COMPLIANT

**Evaluation Details:**
**Compliance:** Yes

**Deviations:**  
- None  

**Suggestions:**  
- None  

**Explanation:**  
The UML diagram includes a data store named `Audit Log` with the technology `Amazon RDS PostgreSQL`, which satisfies the policy requirement for an audit log or persistent data store. The policy explicitly checks for a container with a name containing "audit" or "log" and a technology such as PostgreSQL or RDS, and the `Audit Log` container meets these criteria. Therefore, the architecture is compliant with the policy.

---

#### ARC-POLICY-106 - ❌ NON-COMPLIANT

**Evaluation Details:**
**Compliance:** Yes

**Deviations:**  
- None  

**Suggestions:**  
- None  

**Explanation:**  
The UML diagram adheres to the provided OPA policy (ARC-POLICY-106). The policy mandates that the architecture must declare a secrets management component, either by including "secrets" in the container name or by labeling it with the stereotype `<<secrets_store>>`.  

In the provided UML diagram and input JSON:  
1. The container `AWS Secrets Manager` has "secrets" in its name and is also labeled with the stereotype `<<secrets_store>>`.  
2. This satisfies the policy requirement, ensuring compliance.  

No deviations or fixes are necessary.

---

#### ARC-POLICY-108 - ❌ NON-COMPLIANT

**Evaluation Details:**
**Compliance:** Yes

**Deviations:**  
- None  

**Suggestions:**  
- None  

**Explanation:**  
The UML diagram complies with the provided OPA policy (ARC-POLICY-108). The policy requires at least one container to be labeled as `<<observability>>` or include "cloudwatch" in its name. In the UML diagram, the `Monitoring` container (Amazon CloudWatch) is explicitly labeled as `<<observability>>` and meets the policy requirements. Additionally, the input JSON confirms the presence of this container with the correct stereotype. Therefore, there are no deviations, and the architecture is fully compliant.

---

