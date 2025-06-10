# IaC - Control Policy Deviation Report

**Analysis Date**: 2025-06-10 15:01:50

## 1. IaC Module and Control Policy Mapping

| Iac Module     | Control ID | Implementation Evidence |
|----------------|------------|-------------------------|
| ecs-fargate-module | INFRA-002-control | The aws_lb_listener resource uses HTTPS protocol for secure communication. |
|  | INFRA-005-control | The aws_ecs_service resource is configured to use CloudWatch logs, as indicated by the presence of the network_configuration block which typically includes logging configurations. |
| observability-module | INFRA-005-control | The module includes resources for CloudWatch log group and metric alarm, which captures logs and custom metrics for observability. |
| rds-secrets-module | INFRA-003-control | The aws_db_instance resource has storage_encrypted set to true, ensuring audit logs are stored in encrypted RDS. |
|  | INFRA-004-control | The aws_secretsmanager_secret resource is used to store DB credentials, preventing plaintext secrets in configs. |
|  | SEC-003-control | The aws_secretsmanager_secret resource is used for storing secrets, ensuring they are not hardcoded in code or config. |
| security-module | INFRA-006-control | The security group configuration allows 0.0.0.0/0 in egress, which is a HIGH severity issue according to tfsec results. |

## 2. Control Policy Deviation Analysis

| Iac Module | Control ID | Compliance Status         | Deviation | Suggestion |
|------------|------------|---------------------------|-----------|------------|
| ecs-fargate-module | INFRA-002-control | Complaint✅ | No deviations found | N/A |
| ecs-fargate-module | INFRA-005-control | Non-Compliant❌ | Resource 'aws_ecs_service.app' does not have CloudWatch logs enabled. | Add configuration to enable CloudWatch logs for the ECS service. |
| observability-module | INFRA-005-control | Complaint✅ | No deviations found | N/A |
| rds-secrets-module | INFRA-003-control | Complaint✅ | No deviations found | N/A |
| rds-secrets-module | INFRA-004-control | Complaint✅ | No deviations found | N/A |
| rds-secrets-module | SEC-003-control | Complaint✅ | No deviations found | N/A |
| security-module | INFRA-006-control | Non-Compliant❌ | Security group allows 0.0.0.0/0 for ingress on port 80 in aws_security_group.alb. | Restrict ingress CIDR blocks to specific IP ranges instead of 0.0.0.0/0. |
|  |  |  | Security group allows 0.0.0.0/0 for egress in aws_security_group.ecs, aws_security_group.alb, and aws_security_group.rds. | Restrict egress CIDR blocks to specific IP ranges instead of 0.0.0.0/0. |
|  |  |  | Database encryption not enabled for RDS. | Enable encryption for RDS instances. |
