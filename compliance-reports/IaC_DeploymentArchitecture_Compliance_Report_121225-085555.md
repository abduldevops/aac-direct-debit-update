# Terraform Architecture Compliance Report

**Architecture Diagram**: `DirectDebitMandateUpdateArchitecureDeploymentDiagram.puml`  
**Date**: 2025-12-12  
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
| Label/Annotation Discrepancies                           | No explicit labels or annotations in the diagram are reflected in the Terraform code. |

#### Remediation Steps:

1. **[Define Missing Components]:**
   - Implement resources for MandateUpdateController, MandateUpdateService, and VaultCoreClient in the Terraform code.
   - Ensure these components are part of the ECS task definition and service.

2. **[Define Connections]:**
   - Explicitly define the connections between MandateUpdateController, MandateUpdateService, and VaultCoreClient in the Terraform code, possibly through environment variables or task definitions that reflect these relationships.

3. **[Label/Annotation Consistency]:**
   - Add labels or annotations in the Terraform code to reflect the roles and relationships of the components as depicted in the diagram.
---

### observability-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | CloudWatch                                                     |
| Components Present in PUML Diagram Not in Terraform Code | None                                                           |
| Components Present in Terraform Code Not in PUML Diagram | aws_cloudwatch_metric_alarm                                    |
| Connection Discrepancies                                 | No explicit connections for MandateUpdateService and VaultCoreClient to CloudWatch in Terraform code. |
| Label/Annotation Discrepancies                           | No explicit labels or annotations for logs and metrics in Terraform code. |

#### Remediation Steps:

1. **[Connection Implementation]:**
   - Ensure that the Terraform code explicitly defines the connections for MandateUpdateService and VaultCoreClient to CloudWatch for logs and metrics.
   - Consider using AWS CloudWatch log streams or log groups to capture logs from these services.

2. **[Label/Annotation Consistency]:**
   - Add explicit labels or annotations in the Terraform code to reflect the purpose of logs and metrics as shown in the architecture diagram.
   - Ensure that the log group and metric alarm resources have descriptive tags or names that align with the architecture diagram's annotations.
---

### rds-secrets-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | Secrets Manager, Audit Log (Amazon RDS PostgreSQL)              |
| Components Present in PUML Diagram Not in Terraform Code | None                                                            |
| Components Present in Terraform Code Not in PUML Diagram | aws_db_subnet_group                                             |
| Connection Discrepancies                                 | MandateUpdateController -> Secrets Manager connection not explicitly defined in Terraform code. |
| Label/Annotation Discrepancies                           | None                                                            |

#### Remediation Steps:

1. **[Connection Discrepancies]:**
   - Ensure that the IAM roles and policies are defined in the Terraform code to allow the MandateUpdateController to read secrets from Secrets Manager. This can be done by creating an IAM role with the necessary permissions and associating it with the ECS task definition for the MandateUpdateController.

2. **[Components Present in Terraform Code Not in PUML Diagram]:**
   - Consider updating the architecture diagram to include the `aws_db_subnet_group` if it is a significant part of the architecture, or ensure that its purpose is documented elsewhere if it is an implementation detail.
---

### security-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | None                                                            |
| Components Present in PUML Diagram Not in Terraform Code | Amazon API Gateway                                              |
| Components Present in Terraform Code Not in PUML Diagram | aws_security_group.ecs, aws_security_group.alb, aws_security_group.rds |
| Connection Discrepancies                                 | Amazon API Gateway -> MandateUpdateController (Spring Boot): HTTPS Route not implemented in Terraform code |
| Label/Annotation Discrepancies                           | None                                                            |

#### Remediation Steps:

1. **[Amazon API Gateway Configuration]:**
   - Add a resource block for `aws_api_gateway_rest_api` to define the API Gateway.
   - Ensure the API Gateway is configured to route requests to the MandateUpdateController (Spring Boot) using HTTPS.

2. **[Connection Implementation]:**
   - Implement the connection from Amazon API Gateway to MandateUpdateController using `aws_api_gateway_integration` to define the HTTPS route.

3. **[Security Group Review]:**
   - Review the security groups defined in Terraform to ensure they align with the architecture diagram and intended security posture.
   - Consider adding security group rules that reflect the API Gateway's interaction with ECS components if applicable.
---

