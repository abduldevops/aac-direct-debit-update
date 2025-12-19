# Terraform Architecture Compliance Report

**Architecture Diagram**: `DirectDebitMandateUpdateArchitecureDeploymentDiagram.puml`  
**Date**: 2025-12-19  
## Executive Summary

This report evaluates the compliance of Terraform modules against the architecture diagram. Of the 4 modules evaluated, 0 are compliant and 4 require remediation.

## Module to Component Mapping

The following table shows which components from the architecture diagram are mapped to each module:

| Module Name | Components |
|-------------|------------|
| ecs-fargate-module | MandateUpdateController (Spring Boot), MandateUpdateService (Spring Logic), VaultCoreClient (Adapter) |
| observability-module | CloudWatch |
| rds-secrets-module | Secrets Manager, Audit Log (Amazon RDS PostgreSQL) |
| security-module | Amazon API Gateway |

## Module Compliance Summary

| Module Name | Status | Action Required |
|-------------|--------|----------------|
| ecs-fargate-module | ❌ NON-COMPLIANT | Yes |
| observability-module | ❌ NON-COMPLIANT | Yes |
| rds-secrets-module | ❌ NON-COMPLIANT | Yes |
| security-module | ❌ NON-COMPLIANT | Yes |

## Non-Compliant Modules and Remediation Steps

### ecs-fargate-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | None                                                            |
| Components Present in PUML Diagram Not in Terraform Code | MandateUpdateController, MandateUpdateService, VaultCoreClient  |
| Components Present in Terraform Code Not in PUML Diagram | aws_ecs_cluster, aws_iam_role, aws_iam_role_policy_attachment, aws_ecs_task_definition, aws_lb, aws_lb_target_group, aws_lb_listener, aws_ecs_service |
| Connection Discrepancies                                 | MandateUpdateController -> MandateUpdateService, MandateUpdateService -> VaultCoreClient connections are not explicitly defined in the Terraform code. |
| Label/Annotation Discrepancies                           | No explicit labels or annotations from the diagram are reflected in the Terraform code. |

#### Remediation Steps:

1. **[Component Definition]:**
   - Define ECS task containers for MandateUpdateController, MandateUpdateService, and VaultCoreClient in the `aws_ecs_task_definition` resource.
   - Ensure each component is represented as a separate container definition within the task.

2. **[Connection Implementation]:**
   - Implement service-to-service communication within the ECS task definition to reflect the connections between MandateUpdateController, MandateUpdateService, and VaultCoreClient.
   - Use environment variables or service discovery to facilitate internal communication between these components.

3. **[Label/Annotation Consistency]:**
   - Add labels or tags in the Terraform code to reflect the roles of each component as described in the architecture diagram.
   - Ensure that any specific annotations in the diagram are represented in the Terraform code, possibly through comments or tags.
---

### observability-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | CloudWatch                                                     |
| Components Present in PUML Diagram Not in Terraform Code | None                                                           |
| Components Present in Terraform Code Not in PUML Diagram | aws_cloudwatch_metric_alarm                                    |
| Connection Discrepancies                                 | MandateUpdateService and VaultCoreClient connections to CloudWatch are not explicitly defined in Terraform. |
| Label/Annotation Discrepancies                           | None                                                           |

#### Remediation Steps:

1. **[Connection Discrepancies]:**
   - Define explicit CloudWatch log group and metric stream connections for MandateUpdateService and VaultCoreClient in the Terraform code to ensure logs and metrics are captured as per the architecture diagram.

2. **[Components Present in Terraform Code Not in PUML Diagram]:**
   - Ensure that the aws_cloudwatch_metric_alarm is documented in the architecture diagram if it is a necessary part of the observability module, or remove it if it is not required.
---

### rds-secrets-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | Secrets Manager, Audit Log (Amazon RDS PostgreSQL)              |
| Components Present in PUML Diagram Not in Terraform Code | None                                                            |
| Components Present in Terraform Code Not in PUML Diagram | aws_db_subnet_group.this                                        |
| Connection Discrepancies                                 | MandateUpdateController -> Secrets Manager connection not explicitly defined in Terraform code. |
| Label/Annotation Discrepancies                           | No explicit labels or annotations in the Terraform code for IAM permissions related to Secrets Manager access. |

#### Remediation Steps:

1. **[Define IAM Permissions for Secrets Manager Access]:**
   - Ensure that the IAM role associated with the MandateUpdateController (Spring Boot) has permissions to read from the Secrets Manager.
   - Add an IAM policy resource in the Terraform code to grant the necessary permissions to the IAM role.

2. **[Document Connection in Terraform Code]:**
   - Explicitly document the connection between MandateUpdateController and Secrets Manager in the Terraform code, possibly through comments or additional resources if applicable.

3. **[Review Subnet Group Usage]:**
   - Ensure that the `aws_db_subnet_group.this` is necessary and correctly configured for the RDS instance, and document its purpose in the context of the module.
---

### security-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | None                                                            |
| Components Present in PUML Diagram Not in Terraform Code | Amazon API Gateway                                              |
| Components Present in Terraform Code Not in PUML Diagram | aws_security_group.ecs, aws_security_group.alb, aws_security_group.rds |
| Connection Discrepancies                                 | Amazon API Gateway -> MandateUpdateController (Spring Boot): HTTPS Route is not implemented in Terraform code |
| Label/Annotation Discrepancies                           | No explicit labels or annotations for Amazon API Gateway in Terraform code |

#### Remediation Steps:

1. **[Amazon API Gateway Implementation]:**
   - Add a resource for Amazon API Gateway in the Terraform code.
   - Ensure the API Gateway is configured to route HTTPS requests to the MandateUpdateController (Spring Boot).

2. **[Connection Implementation]:**
   - Implement the connection from Amazon API Gateway to MandateUpdateController in the Terraform code, ensuring it uses HTTPS.

3. **[Review Security Group Usage]:**
   - Verify if the security groups defined in the Terraform code are necessary for the "security-module" or if they should be part of another module.
---

