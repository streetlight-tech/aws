# Streetlight Technologies AWS Management

This repository provides AWS Infrastructure as Code as implemented by Streetlight Technologies. Streetlight leverages a
"hub and stub" account architecture where a all log-ins (interactive or programmatic) and administrative activites
originate in a central "hub" account. Workloads are hosted in a "stub" account and production and non-production 
environments are in different accounts. This architecture can be easily configured using the CloudFormation templates
in this repository.

## CloudFormation Templates

The following paramters are used in various templates in this repository:

- **Environment**: Environment identifier (ex: prod, dev, qa)
- **Identifier**: Unique identifier in PascalCase used for resource names created by the templates (ex: Streetligh)
- **IdentifierLower**: Unique identifire in kebab-case.

The following CloudFormation templates are found in the `/cloudformation` folder:

- `api-gateway-logging.yaml`: Creates an IAM Role to enable API Gateway logging to CloudWatch
- `hub.yaml`: Creates standard resources used in a "hub" account including:
  - `AdminGroup`: IAM Group for administrators
  - `PipelineGroup`: IAM Group for pipeline users
  - `PipelineUser`: IAM user used to run pipelines
  - `PipelineUserAccessKey`: Pipeline user access key
  - `PipelineUserSecretKey`: Secrets Manager entry to store pipeline user access key
- `stub.yaml`: Creates standard resources used in a "stub" account including:
  - `AdminRole`: IAM Role for administrators which can be assumed by the hub administrators group
  - `ArtifactsBucket`: S3 Bucket for pipeline artifacts with optional expiration
  - `ArtifactsBucketPolicy`: S3 policy to allow CloudFormation to access the bucket (used for AWS SAM)
  - `CloudFormationRole`: IAM role for CloudFormation to assume which grants access to manage serverless resources
  - `PipelineRole`: IAM role assumed by pipeline users to allow CloudFormation actions

