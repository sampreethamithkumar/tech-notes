# **OpenID Connect (OIDC) and AWS Integration with GitHub Actions**  

## **What is OIDC?**  
OIDC (OpenID Connect) is an authentication protocol built on top of OAuth 2.0. It allows applications to verify identities and obtain identity tokens securely.  

---

## **How OIDC Works in AWS?**  
AWS allows external identity providers (IdPs) like GitHub to authenticate and grant temporary credentials to assume roles in AWS. This eliminates the need for storing long-lived IAM access keys.  

### **OIDC Authentication Flow in AWS & GitHub Actions**  
1. **GitHub requests a token** from its built-in OIDC provider.  
2. **GitHub OIDC provider sends a signed JWT** (JSON Web Token) with claims (e.g., repo name, workflow).  
3. **AWS validates the JWT** against the GitHub OIDC provider.  
4. **AWS grants temporary credentials** based on IAM role permissions.  
5. **GitHub Actions assume the AWS role** and execute actions securely.  

---

## **Setting Up OIDC in AWS Using CloudFormation**  

### **Step 1: Deploy CloudFormation Stack**  

Use the following **CloudFormation template** to create the **OIDC Identity Provider** and an **IAM Role** for GitHub Actions.

#### **`oidc-github-aws.yaml`**
```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "Setup GitHub OIDC Provider and IAM Role for AWS Authentication"

Parameters:
  GitHubOrgRepo:
    Description: "GitHub Organization and Repository (e.g., my-org/my-repo)"
    Type: String

Resources:
  GitHubOIDCProvider:
    Type: AWS::IAM::OIDCProvider
    Properties:
      Url: "https://token.actions.githubusercontent.com"
      ClientIdList:
        - "sts.amazonaws.com"
      ThumbprintList:
        - "6938fd4d98bab03faadb97b34396831e3780aea1"  # Valid as of AWS documentation

  GitHubActionsRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: "GitHubActionsOIDCRole"
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: "Allow"
            Principal:
              Federated: !Ref GitHubOIDCProvider
            Action: "sts:AssumeRoleWithWebIdentity"
            Condition:
              StringLike:
                "token.actions.githubusercontent.com:sub": !Sub "repo:${GitHubOrgRepo}:ref:refs/heads/main"
              StringEquals:
                token.actions.githubusercontent.com:aud: sts.amazonaws.com   
      Policies:
        - PolicyName: "GitHubActionsPermissions"
          PolicyDocument:
            Version: "2012-10-17"
            Statement:
              - Effect: "Allow"
                Action:
                  - "s3:ListBucket"
                  - "s3:GetObject"
                  - "s3:PutObject"
                Resource: "*"

Outputs:
  GitHubActionsRoleArn:
    Description: "IAM Role ARN for GitHub Actions"
    Value: !GetAtt GitHubActionsRole.Arn
```

To understand thumbprint [click here](./thumbprint.md)

### **Step 2: Deploy the Stack**
Run the following AWS CLI command, replacing `<your-org>/<your-repo>`:

```sh
aws cloudformation deploy \
  --template-file oidc-github-aws.yaml \
  --stack-name GitHubOIDCStack \
  --parameter-overrides GitHubOrgRepo=<your-org>/<your-repo> \
  --capabilities CAPABILITY_NAMED_IAM
```

This will:

✅ Create the OIDC Provider for GitHub  
✅ Create an IAM Role with **GitHub repository-specific** access  
✅ Restrict access only to the `main` branch of the repo  

---

## **Step 3: Use the IAM Role in GitHub Actions**  
Modify your GitHub Actions workflow (`.github/workflows/deploy.yml`):  

```yaml
name: Deploy to AWS

on:
  push:
    branches:
      - main

permissions:
  id-token: write
  contents: read

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          role-to-assume: arn:aws:iam::<AWS_ACCOUNT_ID>:role/GitHubActionsOIDCRole
          aws-region: us-east-1

      - name: Deploy
        run: |
          aws s3 ls
```

---

## **How OIDC Avoids Long-Lived Credentials**  
Before OIDC, GitHub Actions used **IAM user access keys**, which had several risks:  
❌ **Hardcoded Secrets** – Storing credentials in GitHub secrets was a security risk.  
❌ **Key Rotation** – Keys had to be manually rotated.  
❌ **Risk of Exposure** – If a secret leaked, attackers could misuse AWS resources.  

✅ **With OIDC, temporary credentials are dynamically assigned**, eliminating the need for long-lived access keys!  

---

## **Benefits of Using OIDC for AWS Authentication**  
✔ **No Need for IAM User Credentials** – Uses short-lived tokens instead.  
✔ **More Secure** – Reduces the risk of leaked credentials.  
✔ **Granular Access Control** – Limits access to specific GitHub repositories and branches.  
✔ **Automated and Scalable** – No need to manually rotate secrets.  

---

🚀 This CloudFormation-based approach provides a **secure, automated, and scalable** AWS authentication mechanism for GitHub Actions using OIDC.  

---
Ref
- https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services
- https://github.blog/changelog/2023-06-27-github-actions-update-on-oidc-integration-with-aws/


