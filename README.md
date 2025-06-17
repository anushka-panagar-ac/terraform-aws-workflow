# Pipeline for Terraform and AWS Services
This workflow performs security scans using AWS CodeGuru Security and uploads the scan results as an artifact.

#### Key Features:
- Assumes an AWS role for authentication.
- Runs CodeGuru Security to analyze the codebase.
- Prints findings and uploads the security scan report.

### 2. `deployment.yml`
This workflow builds, tests, and deploys the application to AWS services.

#### Key Features:
- **Environment Setup**: Determines the deployment environment (`Prod`, `Uat`, or `Dev`) based on the branch.
- **Build and Push to ECR**:
    - Configures AWS credentials.
    - Builds and pushes Docker images to Amazon ECR.
    - Runs tests using Gradle.
    - Applies Terraform configurations for ECR setup.
- **Deploy to ECS**:
    - Applies Terraform configurations for ECS deployment using the Docker image.

## Environment Variables

The workflows use the following environment variables:
- `AWS_REGION`: AWS region for deployment (e.g., `us-west-2`).
- `AWS_ROLE_ARN`: ARN of the AWS role to assume.
- `TERRAFORM_BACKEND_PATH`: Path to the Terraform backend configuration.
- `TERRAFORM_TFVARS_PATH`: Path to the Terraform variables file.
- `AWS_URL`: URL for AWS services.
- `JAVA_VERSION`: Java version for the application build.

## Requirements

- **Terraform**: Version 1.7.5
- **Java**: Version 21
- **AWS CLI**: Configured with appropriate permissions.
- **Docker**: For building and pushing images.

## Usage

### Triggering Workflows
- **`validate-plan-document-scan.yml`**: Triggered automatically or manually for security scans.
- **`deployment.yml`**: Triggered on `push` events to `release/*` or `develop` branches, or manually via `workflow_dispatch`.

### Running Locally
1. Clone the repository.
2. Set up required environment variables.
3. Run Terraform commands for infrastructure provisioning:
   ```bash
   cd terraform
   terraform init
   terraform plan
   terraform apply