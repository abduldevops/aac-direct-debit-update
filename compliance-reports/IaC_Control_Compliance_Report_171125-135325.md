# IaC - Control Policy Deviation Report

**Analysis Date**: 2025-11-17 13:53:23

## 1. IaC Module and Control Policy Mapping

| Iac Module     | Control ID | Implementation Evidence |
|----------------|------------|-------------------------|
| ecs-fargate-module | INFRA-002-control | The aws_lb_listener resource is configured to use HTTPS protocol on port 443, ensuring secure communication. |
|  | INFRA-005-control | The aws_ecs_service resource is configured with a network_configuration block, but there is no explicit evidence of CloudWatch logs being enabled in the provided code. However, the use of Fargate implies potential integration with CloudWatch for logs and metrics. |
| observability-module | INFRA-005-control | The module includes resources for CloudWatch log group and metric alarm, which aligns with the requirement to capture logs and custom metrics for observability. |
| rds-secrets-module | INFRA-003-control | The RDS instance 'aws_db_instance.audit' has 'storage_encrypted' set to true, ensuring audit logs are stored in encrypted RDS. |
|  | INFRA-004-control | The module uses 'aws_secretsmanager_secret' to store DB credentials, ensuring sensitive credentials are not stored in plain text. |
|  | SEC-003-control | The module uses AWS Secrets Manager to store database credentials, preventing hardcoded secrets in the code. |
| security-module | INFRA-006-control | The security group configurations allow 0.0.0.0/0 for egress, which is a common tfsec finding for high/critical issues. This indicates awareness of potential tfsec checks, although not explicitly passing them. |

## 2. Control Policy Deviation Analysis

| Iac Module | Control ID | Compliance Status         | Deviation | Suggestion |
|------------|------------|---------------------------|-----------|------------|
| ecs-fargate-module | INFRA-002-control | Complaint✅ | No deviations found | N/A |
| ecs-fargate-module | INFRA-005-control | Non-Compliant❌ | The Terraform code does not explicitly enable CloudWatch logs for the ECS service. | Add a configuration to enable CloudWatch logs for the ECS service, ensuring logs are pushed to CloudWatch. |
| observability-module | INFRA-005-control | Complaint✅ | No deviations found | N/A |
| rds-secrets-module | INFRA-003-control | Complaint✅ | No deviations found | N/A |
| rds-secrets-module | INFRA-004-control | Complaint✅ | No deviations found | N/A |
| rds-secrets-module | SEC-003-control | Complaint✅ | No deviations found | N/A |
| security-module | INFRA-006-control | Non-Compliant❌ | Security group allows 0.0.0.0/0 for egress traffic, which is flagged as HIGH severity by tfsec. | Restrict egress traffic to specific IP ranges instead of 0.0.0.0/0. |
|  |  |  | Database encryption not enabled, which is flagged as CRITICAL severity by tfsec. | Enable encryption for the database resources. |
