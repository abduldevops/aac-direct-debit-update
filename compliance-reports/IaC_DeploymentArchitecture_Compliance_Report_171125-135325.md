# Terraform Architecture Compliance Report

**Architecture Diagram**: `DirectDebitMandateUpdateArchitecureDeploymentDiagram.puml`  
**Date**: 2025-11-17  
## Executive Summary

This report evaluates the compliance of Terraform modules against the architecture diagram. Of the 4 modules evaluated, 1 is compliant and 3 require remediation.

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
| ecs-fargate-module | ✅ COMPLIANT | No |
| observability-module | ❌ NON-COMPLIANT | Yes |
| rds-secrets-module | ❌ NON-COMPLIANT | Yes |
| security-module | ❌ NON-COMPLIANT | Yes |

## Non-Compliant Modules and Remediation Steps

### observability-module

| Category                                              | Details                                                         |
|----------------------------------------------------------|----------------------------------------------------------------|
| Components Present in PUML Diagram and Terraform Code    | CloudWatch                                                     |
| Components Present in PUML Diagram Not in Terraform Code | None                                                           |
| Components Present in Terraform Code Not in PUML Diagram | aws_cloudwatch_metric_alarm.high_cpu                           |
| Connection Discrepancies                                 | MandateUpdateService and VaultCoreClient connections to CloudWatch are not explicitly defined in Terraform. |
| Label/Annotation Discrepancies                           | None                                                           |

#### Remediation Steps:

1. **[Connection Implementation]:**
   - Ensure that the connections from MandateUpdateService and VaultCoreClient to CloudWatch are explicitly defined in the Terraform code. This could involve setting up appropriate IAM roles and policies to allow these services to send logs and metrics to CloudWatch.

2. **[Resource Alignment]:**
   - Review the necessity of the `aws_cloudwatch_metric_alarm.high_cpu` resource in the context of the observability module. If it is not part of the intended architecture, consider removing it or documenting its purpose in relation to the module.
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

1. **[Connection Discrepancy]:**
   - Ensure that the IAM roles and policies are defined in the Terraform code to allow the MandateUpdateController to read secrets from Secrets Manager. This can be done by creating an IAM role with the necessary permissions and associating it with the ECS task running the MandateUpdateController.

2. **[Components Present in Terraform Code Not in PUML Diagram]:**
   - Consider updating the architecture diagram to include the `aws_db_subnet_group` if it is a significant part of the architecture, or ensure that its presence is justified in the Terraform code as a necessary infrastructure component.
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

1. **[Add Amazon API Gateway Resource]:**
   - Define an `aws_api_gateway_rest_api` resource in the Terraform code to represent the Amazon API Gateway component.
   - Ensure the API Gateway is configured to route requests to the MandateUpdateController (Spring Boot) component.

2. **[Implement Connection]:**
   - Implement the connection from Amazon API Gateway to MandateUpdateController (Spring Boot) using an `aws_api_gateway_integration` resource.
   - Ensure the integration type is set to HTTP and the endpoint URL points to the ECS Fargate service hosting the MandateUpdateController.

3. **[Review Security Group Configuration]:**
   - Verify if the existing security groups (ecs, alb, rds) are necessary for the security-module or if they should be part of another module.
   - If necessary, adjust the security group configurations to ensure they align with the architectural design and security best practices.
---

## Compliant Modules

### ecs-fargate-module
✅ All diagram components and connections are correctly implemented in code.

