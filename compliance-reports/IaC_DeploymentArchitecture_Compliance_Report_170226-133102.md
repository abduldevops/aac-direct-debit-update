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
| Label/Annotation Discrepancies                           | No explicit labels or annotations for connections like "Delegate Request" or "External Call" in Terraform code. |

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
| Components Present in PUML Diagram and Terraform Code    | CloudWatch (Log Group, Metric Alarm)                           |
| Components Present in PUML Diagram Not in Terraform Code | None                                                           |
| Components Present in Terraform Code Not in PUML Diagram | None                                                           |
| Connection Discrepancies                                 | MandateUpdateService -> CloudWatch (Logs, Metrics) and VaultCoreClient -> CloudWatch (Retry Logs) are not explicitly implemented in Terraform code. No resources or configurations link ECS services to CloudWatch for logs or metrics. |
| Label/Annotation Discrepancies                           | The diagram specifies "Logs, Metrics" and "Retry Logs" for CloudWatch connections, but these labels are not reflected in Terraform code. No explicit tags or annotations for these connections exist. |

#### Remediation Steps:

1. **[Action Area 1]: Implement ECS Service Logging to CloudWatch**
   - Add `aws_ecs_task_definition` resource to define ECS tasks with logging configuration pointing to the CloudWatch Log Group (`aws_cloudwatch_log_group.app_logs`).
   - Ensure the ECS service is configured to send logs to the CloudWatch Log Group.

2. **[Action Area 2]: Configure Metrics for ECS Services**
   - Add `aws_cloudwatch_metric_alarm` resources for ECS service-specific metrics (e.g., Retry Logs, Application Metrics).
   - Use appropriate dimensions (`ClusterName`, `ServiceName`) to link ECS services to CloudWatch metrics.

3. **[Action Area 3]: Add Explicit Tags or Annotations**
   - Include tags or annotations in Terraform code to reflect the labels "Logs, Metrics" and "Retry Logs" as described in the diagram.
   - Ensure these tags are applied to relevant resources (e.g., Log Groups, Metric Alarms).
---

### rds-secrets-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | Secrets Manager, Audit Log (Amazon RDS PostgreSQL)             |
| Components Present in PUML Diagram Not in Terraform Code | None                                                           |
| Components Present in Terraform Code Not in PUML Diagram | aws_db_subnet_group.this                                       |
| Connection Discrepancies                                 | MandateUpdateController -> Secrets Manager connection is not explicitly implemented in Terraform code (IAM role/policy missing). MandateUpdateService -> Audit Log connection is not explicitly implemented in Terraform code (JDBC configuration missing). |
| Label/Annotation Discrepancies                           | No explicit labels for "Reads Secrets (IAM)" or "Audit JDBC Write" in Terraform code. |

#### Remediation Steps:

1. **[IAM Role/Policy for Secrets Manager Access]:**
   - Create an IAM role and policy that allows MandateUpdateController (Spring Boot) to read secrets from Secrets Manager.
   - Attach the IAM role to the ECS Fargate task definition for MandateUpdateController.

2. **[JDBC Configuration for Audit Log Access]:**
   - Ensure MandateUpdateService (Spring Logic) has the necessary JDBC configuration to connect to the Audit Log (Amazon RDS PostgreSQL) using credentials stored in Secrets Manager.
   - Add a mechanism to retrieve secrets from Secrets Manager and inject them into the JDBC configuration.

3. **[Explicit Labels/Annotations]:**
   - Add comments or outputs in Terraform code to explicitly reflect the "Reads Secrets (IAM)" and "Audit JDBC Write" connections for better traceability and alignment with the architecture diagram.

4. **[Subnet Group Documentation]:**
   - Document the purpose of `aws_db_subnet_group.this` in relation to the Audit Log (Amazon RDS PostgreSQL) to ensure alignment with the architecture diagram.
---

### security-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | Amazon API Gateway                                              |
| Components Present in PUML Diagram Not in Terraform Code | None                                                           |
| Components Present in Terraform Code Not in PUML Diagram | aws_security_group.ecs, aws_security_group.alb, aws_security_group.rds |
| Connection Discrepancies                                 | The connection "Amazon API Gateway -> MandateUpdateController (Spring Boot): HTTPS Route" is not explicitly implemented in the Terraform code. There is no resource or configuration to define this route. |
| Label/Annotation Discrepancies                           | No explicit labels or annotations for "Amazon API Gateway" or its connection to "MandateUpdateController" are reflected in the Terraform code. |

#### Remediation Steps:

1. **Review Architecture Diagram:**
   - Carefully analyze the diagram components related to this module
   - Compare with implemented Terraform code

2. **Implement Missing Components/Connections:**
   - Add the necessary Terraform resources
   - Configure proper connections between components


---

