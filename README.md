# aws-oidc-new

## Overview
This project demonstrates how to add an OpenID Connect (OIDC) identity provider to AWS Identity and Access Management (IAM) using GitHub as the identity provider.

## Adding the Identity Provider to AWS
To add the GitHub OIDC provider to IAM, follow the instructions provided in the [AWS documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html).

### Configuration Details
- **Provider URL**: `https://token.actions.githubusercontent.com`
- **Audience**: `sts.amazonaws.com` (use this if you are using the official GitHub Action)

## Steps to Configure
1. **Navigate to IAM**:
   - Go to the AWS Management Console.
   - Open the IAM service.

2. **Add a new identity provider**:
   - Select "Identity providers" from the sidebar.
   - Click on "Add provider".

3. **Set the provider type**:
   - Choose "OpenID Connect (OIDC)".

4. **Fill in the details**:
   - For **Provider URL**, enter `https://token.actions.githubusercontent.com`.
   - For **Audience**, enter `sts.amazonaws.com`.

5. **Create the provider**:
   - Follow the remaining prompts to complete the setup.

6. **Create a role for the provider**:
   - Provide a role (s3FullAccess) and mention the GitHub organizaition, Repository and Branch details.
     
## Usage
After adding the GitHub OIDC provider, you can configure your GitHub Actions workflows to assume roles in your AWS account. This allows your workflows to access AWS resources securely without needing long-lived AWS credentials.

### Example GitHub Action Workflow
```yaml
name: AWS OIDC Example

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          role-to-assume: arn:aws:iam::123456789012:role/OIDCRole
          aws-region: us-west-2

      - name: Deploy to AWS
        run: |
          aws s3 cp my-app s3://my-bucket/ --recursive


