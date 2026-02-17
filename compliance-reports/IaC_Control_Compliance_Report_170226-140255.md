# IaC - Control Policy Deviation Report

**Analysis Date**: 2026-02-17 14:02:53

## 1. IaC Module and Control Policy Mapping

| Iac Module     | Control ID | Implementation Evidence |
|----------------|------------|-------------------------|
| ecs-fargate-module | INFRA-002-control | The `aws_lb_listener` resource enforces HTTPS protocol on port 443, ensuring secure communication. |
|  | INFRA-005-control | The `aws_ecs_task_definition` and `aws_ecs_service` resources include configurations for ECS tasks and services, which can be integrated with CloudWatch for logs and metrics. However, explicit CloudWatch configuration is not shown in the code. |
|  | SEC-001-control | The `aws_lb_target_group` resource uses HTTPS protocol for the target group, which implies secure communication for REST endpoints. |
| observability-module | INFRA-005-control | The module explicitly creates a CloudWatch Log Group (`aws_cloudwatch_log_group.app_logs`) to capture logs and a CloudWatch Metric Alarm (`aws_cloudwatch_metric_alarm.high_cpu`) for custom metrics, fulfilling observability requirements. |
| rds-secrets-module | INFRA-003-control | The RDS instance 'aws_db_instance.audit' has 'storage_encrypted = true', ensuring audit logs are stored in encrypted RDS. |
|  | INFRA-004-control | The module uses 'aws_secretsmanager_secret' and 'aws_secretsmanager_secret_version' to store and manage database credentials securely. |
|  | SEC-003-control | Database credentials (username and password) are stored in AWS Secrets Manager using 'aws_secretsmanager_secret' and 'aws_secretsmanager_secret_version', preventing hardcoded secrets. |
| security-module | INFRA-003-control | The module defines a security group for RDS (aws_security_group.rds) with ingress rules allowing PostgreSQL traffic from ECS, which aligns with the requirement to secure database access. However, encryption of RDS storage is not explicitly configured in this module. |
|  | INFRA-004-control | The module does not hardcode any secrets or credentials in the Terraform code. Instead, it uses variables (e.g., var.alb_sg_id) to reference sensitive information, which can be managed securely outside the code. |

## 2. Control Policy Deviation Analysis

| Iac Module | Control ID | Compliance Status         | Deviation | Suggestion |
|------------|------------|---------------------------|-----------|------------|
| ecs-fargate-module | INFRA-002-control | Complaint✅ | No deviations found | N/A |
| ecs-fargate-module | INFRA-005-control | Non-Compliant❌ | The ECS service defined in the Terraform code does not explicitly enable CloudWatch logs or custom metrics. The policy requires all applications to push logs and metrics to CloudWatch for observabilit | Add the `aws_cloudwatch_log_group` resource to define a log group and configure the ECS task definition to use it by specifying the `logConfiguration` block within the `container_definitions`. Ensure  |
| ecs-fargate-module | SEC-001-control | Non-Compliant❌ | The endpoint '/api/v1/public/info' in the OpenAPI specification is missing authentication requirements. | Add a 'security' attribute to the '/api/v1/public/info' endpoint in the OpenAPI specification to enforce authentication. |
| observability-module | INFRA-005-control | Non-Compliant❌ | The Terraform code does not include a resource of type 'aws_ecs_service' with 'cloudwatch_logs_enabled' and 'custom_metrics_enabled' attributes. | Add an 'aws_ecs_service' resource with 'cloudwatch_logs_enabled = true' and 'custom_metrics_enabled = true' attributes to ensure compliance with the policy. |
| rds-secrets-module | INFRA-003-control | Non-Compliant❌ | The RDS instance 'audit-db' does not match the required name 'audit-logs-db' as specified in the policy. | Rename the RDS instance to 'audit-logs-db' to align with the policy requirements. |
|  |  |  | The RDS instance 'audit-db' is missing the required 'storage_encrypted' attribute set to true in the JSON configuration. | Ensure the 'storage_encrypted' attribute is explicitly set to true for the RDS instance 'audit-db'. |
| rds-secrets-module | INFRA-004-control | Complaint✅ | No deviations found | N/A |
| rds-secrets-module | SEC-003-control | Complaint✅ | No deviations found | N/A |
| security-module | INFRA-003-control | Non-Compliant❌ | The Terraform code does not define an RDS instance named 'audit-logs-db' with encryption enabled. | Add an RDS instance resource with the name 'audit-logs-db' and ensure the 'storage_encrypted' attribute is set to 'true'. |
| security-module | INFRA-004-control | Non-Compliant❌ | Sensitive credentials such as database credentials are not stored in AWS Secrets Manager. The Terraform code does not define any 'aws_secretsmanager_secret' resources for storing sensitive information | Add an 'aws_secretsmanager_secret' resource to store sensitive credentials such as database credentials securely. |
