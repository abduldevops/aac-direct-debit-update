# Terraform Architecture Compliance Report

**Architecture Diagram**: `DirectDebitMandateUpdateArchitecureDeploymentDiagram.puml`  
**Date**: 2026-02-17  
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
| Components Present in PUML Diagram and Terraform Code    | MandateUpdateController, MandateUpdateService, VaultCoreClient |
| Components Present in PUML Diagram Not in Terraform Code | None                                                           |
| Components Present in Terraform Code Not in PUML Diagram | aws_lb, aws_lb_target_group, aws_lb_listener                  |
| Connection Discrepancies                                 | VaultCoreClient -> Thought Machine Vault Core API connection is not explicitly defined in Terraform code. |
| Label/Annotation Discrepancies                           | No explicit labels or annotations for connections (e.g., "Delegate Request", "External Call") are reflected in the Terraform code. |

#### Remediation Steps:

1. **Review Architecture Diagram:**
   - Carefully analyze the diagram components related to this module
   - Compare with implemented Terraform code

2. **Implement Missing Components/Connections:**
   - Add the necessary Terraform resources
   - Configure proper connections between components


---

### observability-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | CloudWatch                                                     |
| Components Present in PUML Diagram Not in Terraform Code | None                                                           |
| Components Present in Terraform Code Not in PUML Diagram | aws_cloudwatch_metric_alarm (high_cpu)                         |
| Connection Discrepancies                                 | MandateUpdateService -> CloudWatch (Logs, Metrics) and VaultCoreClient -> CloudWatch (Retry Logs) are not explicitly implemented in Terraform code. No resources or configurations link ECS services to CloudWatch for logs or metrics. |
| Label/Annotation Discrepancies                           | The diagram specifies "Logs, Metrics" and "Retry Logs" for CloudWatch connections, but these annotations are not reflected in the Terraform code. |

#### Remediation Steps:

1. **[Action Area 1]: Implement ECS Service Logging to CloudWatch**
   - Add `aws_ecs_task_definition` resource to define ECS tasks with logging configuration pointing to the CloudWatch log group (`aws_cloudwatch_log_group.app_logs`).
   - Ensure ECS service definitions include `logConfiguration` for CloudWatch integration.

2. **[Action Area 2]: Configure Metrics Collection for ECS Services**
   - Use `aws_cloudwatch_metric_alarm` to monitor ECS service metrics (e.g., CPUUtilization, MemoryUtilization) and ensure these metrics are tied to the ECS services (`MandateUpdateService` and `VaultCoreClient`).
   - Add necessary IAM permissions for ECS tasks to write logs and metrics to CloudWatch.

3. **[Action Area 3]: Annotate Connections in Terraform Code**
   - Include comments or outputs in Terraform code to explicitly document the connections between ECS services (`MandateUpdateService` and `VaultCoreClient`) and CloudWatch for logs and metrics.
---

### rds-secrets-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | Secrets Manager, Audit Log (Amazon RDS PostgreSQL)             |
| Components Present in PUML Diagram Not in Terraform Code | None                                                           |
| Components Present in Terraform Code Not in PUML Diagram | None                                                           |
| Connection Discrepancies                                 | MandateUpdateController -> Secrets Manager connection is not explicitly implemented in Terraform code (IAM permissions for Secrets Manager access are missing). <br> MandateUpdateService -> Audit Log connection is not explicitly implemented in Terraform code (no JDBC configuration or IAM role for database access). |
| Label/Annotation Discrepancies                           | Secrets Manager resource lacks explicit tags or description matching the diagram annotations. <br> Audit Log (Amazon RDS PostgreSQL) lacks explicit subnet and availability zone annotations in Terraform code. |

#### Remediation Steps:

1. **[IAM Permissions for Secrets Manager Access]:**
   - Add an IAM role or policy to allow `MandateUpdateController` (Spring Boot) to access Secrets Manager for reading secrets.
   - Ensure the IAM role is attached to the ECS Fargate task definition.

2. **[Database Access Configuration]:**
   - Add IAM role or policy to allow `MandateUpdateService` (Spring Logic) to access the RDS database securely.
   - Include JDBC connection details in the application configuration (e.g., environment variables or injected secrets).

3. **[Subnet and Availability Zone Configuration]:**
   - Ensure the RDS instance is deployed in private subnets across multiple availability zones for high availability.
   - Verify that the subnet IDs provided in `aws_db_subnet_group` correspond to private subnets.

4. **[Tags and Annotations]:**
   - Add tags to Secrets Manager and RDS resources that match the diagram annotations (e.g., "Component: Secrets Manager", "Component: Audit Log").
   - Include descriptions in Terraform code that align with the diagram labels for clarity.

5. **[Explicit Connections in Code]:**
   - Document the connections between `MandateUpdateController` and Secrets Manager, and `MandateUpdateService` and Audit Log in Terraform code (e.g., IAM roles, environment variables, or injected secrets).
---

### security-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | Amazon API Gateway                                              |
| Components Present in PUML Diagram Not in Terraform Code | None                                                           |
| Components Present in Terraform Code Not in PUML Diagram | aws_security_group.ecs, aws_security_group.alb, aws_security_group.rds |
| Connection Discrepancies                                 | The connection "Amazon API Gateway -> MandateUpdateController (Spring Boot): HTTPS Route" is not explicitly implemented in the Terraform code. There is no resource or configuration that defines this route. |
| Label/Annotation Discrepancies                           | No explicit labels or annotations for "Amazon API Gateway" in the Terraform code to match the diagram. |

#### Remediation Steps:

1. **[Define API Gateway Route]:**
   - Add a Terraform resource for the API Gateway route that connects to the MandateUpdateController (Spring Boot) service. For example, use the `aws_api_gateway_integration` and `aws_api_gateway_method` resources to define the HTTPS route.

2. **[Add Labels/Annotations]:**
   - Include tags or metadata in the Terraform configuration for the API Gateway resource to reflect its role as defined in the architecture diagram (e.g., "Component: Amazon API Gateway").

3. **[Review Unused Security Groups]:**
   - Evaluate the necessity of the `aws_security_group.ecs`, `aws_security_group.alb`, and `aws_security_group.rds` resources in the context of the "security-module". If they are unrelated to the module's scope, consider moving them to a different module or removing them.
---

