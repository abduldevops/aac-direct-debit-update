# Terraform Architecture Compliance Report

**Architecture Diagram**: `DirectDebitMandateUpdateArchitecureDeploymentDiagram.puml`  
**Date**: 2025-12-01  
## Executive Summary

This report evaluates the compliance of Terraform modules against the architecture diagram. Of the 4 modules evaluated, 0 are compliant and 4 require remediation.

## Module to Component Mapping

The following table shows which components from the architecture diagram are mapped to each module:

| Module Name | Components |
|-------------|------------|
| ecs-fargate-module | ECS Fargate Cluster, MandateUpdateController (Spring Boot), MandateUpdateService (Spring Logic), VaultCoreClient (Adapter) |
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
| Components Present in PUML Diagram and Terraform Code    | ECS Fargate Cluster                                             |
| Components Present in PUML Diagram Not in Terraform Code | MandateUpdateController (Spring Boot), MandateUpdateService (Spring Logic), VaultCoreClient (Adapter) |
| Components Present in Terraform Code Not in PUML Diagram | aws_lb, aws_lb_target_group, aws_lb_listener, aws_iam_role, aws_iam_role_policy_attachment, aws_ecs_task_definition, aws_ecs_service |
| Connection Discrepancies                                 | The connections between MandateUpdateController, MandateUpdateService, and VaultCoreClient are not explicitly defined in the Terraform code. |
| Label/Annotation Discrepancies                           | The Terraform code does not include explicit labels or annotations for the components as described in the diagram. |

#### Remediation Steps:

1. **[Component Definition]:**
   - Define resources or modules for MandateUpdateController, MandateUpdateService, and VaultCoreClient in the Terraform code.
   - Ensure these components are represented as ECS task definitions or containers within the ECS service.

2. **[Connection Implementation]:**
   - Implement the connections between MandateUpdateController, MandateUpdateService, and VaultCoreClient in the Terraform code, possibly through environment variables or task definitions that define service interactions.

3. **[Label/Annotation Consistency]:**
   - Add labels or annotations in the Terraform code to reflect the roles and interactions of the components as described in the diagram. This could involve using tags or comments to clarify the purpose of each resource.
---

### observability-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | CloudWatch                                                     |
| Components Present in PUML Diagram Not in Terraform Code | None                                                           |
| Components Present in Terraform Code Not in PUML Diagram | aws_cloudwatch_metric_alarm (high_cpu)                         |
| Connection Discrepancies                                 | MandateUpdateService and VaultCoreClient connections to CloudWatch are not explicitly defined in Terraform. |
| Label/Annotation Discrepancies                           | No explicit labels or annotations for CloudWatch connections in Terraform. |

#### Remediation Steps:

1. **[Connection Implementation]:**
   - Ensure that the connections from MandateUpdateService and VaultCoreClient to CloudWatch are explicitly defined in the Terraform code. This could involve setting up appropriate IAM roles and policies to allow these services to send logs and metrics to CloudWatch.

2. **[Label/Annotation Consistency]:**
   - Add explicit labels or annotations in the Terraform code to reflect the connections and purposes as described in the architecture diagram, ensuring clarity and consistency between the diagram and the code.
---

### rds-secrets-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | Secrets Manager, Audit Log (Amazon RDS PostgreSQL)              |
| Components Present in PUML Diagram Not in Terraform Code | None                                                            |
| Components Present in Terraform Code Not in PUML Diagram | aws_db_subnet_group, aws_secretsmanager_secret_version          |
| Connection Discrepancies                                 | MandateUpdateController -> Secrets Manager connection not explicitly defined in Terraform code. |
| Label/Annotation Discrepancies                           | No explicit IAM role or policy annotations for Secrets Manager access in Terraform code. |

#### Remediation Steps:

1. **[Define IAM Role for Secrets Access]:**
   - Create an IAM role with permissions to access the Secrets Manager.
   - Attach the IAM role to the ECS Fargate task definition for MandateUpdateController.

2. **[Explicitly Define Connections]:**
   - Ensure that the connection from MandateUpdateController to Secrets Manager is explicitly defined in the Terraform code, possibly through IAM policies or task definitions.

3. **[Review Subnet and Security Group Configuration]:**
   - Ensure that the RDS instance is placed in private subnets and that the security group allows necessary traffic from the ECS Fargate cluster.
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

1. **[Add Missing Component]:**
   - Implement a Terraform resource for Amazon API Gateway to reflect its presence in the architecture diagram.

2. **[Implement Missing Connection]:**
   - Define the connection from Amazon API Gateway to MandateUpdateController (Spring Boot) using an appropriate Terraform resource or module to establish the HTTPS Route.

3. **[Review Security Groups]:**
   - Ensure that the security groups defined in the Terraform code are necessary and correctly associated with the components in the architecture diagram. If they are not part of the "security-module", consider moving them to the appropriate module.
---

