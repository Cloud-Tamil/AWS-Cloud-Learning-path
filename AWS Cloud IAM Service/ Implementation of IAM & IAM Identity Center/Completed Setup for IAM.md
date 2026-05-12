# 🔐 AWS IAM & IAM Identity Center Implementation Guide

<p align="center">
  <img src="https://img.shields.io/badge/AWS-IAM%20Identity%20Center-orange?style=for-the-badge&logo=amazonaws" />
  <img src="https://img.shields.io/badge/Security-SSO-blue?style=for-the-badge&logo=amazonaws" />
  <img src="https://img.shields.io/badge/Cloud-AWS-yellow?style=for-the-badge&logo=amazonaws" />
</p>

<p align="center">
  <b>Enterprise-grade AWS IAM Identity Center (AWS SSO) implementation guide</b>
</p>

---

## 📖 Overview

> Modern AWS enterprise environments use IAM Identity Center (AWS SSO) to provide centralized authentication, secure temporary credentials, and scalable multi-account access management.

This architecture demonstrates how users authenticate using a corporate identity provider and securely access multiple AWS accounts through permission sets and IAM roles.

---

This repository explains how to implement:

* AWS IAM Identity Center (AWS SSO)
* AWS Organizations
* Multi-account AWS architecture
* Permission Sets & IAM Roles
* RBAC (Role-Based Access Control)
* Secure Console & CLI access
* Enterprise governance & security

---

## 🌍 What You Will Build

✅ AWS Organizations multi-account setup
✅ Centralized authentication using IAM Identity Center
✅ Single Sign-On (SSO) for AWS Console & CLI
✅ Group-based RBAC model
✅ Automatic IAM role provisioning
✅ Temporary credential workflow
✅ Enterprise governance & auditing

---

## 🧠 Learning Goals

After completing this guide, you will understand:

* Difference between IAM vs IAM Identity Center
* Enterprise AWS authentication flow
* How SSO works internally
* Permission Sets & IAM Roles
* Temporary credential security
* Multi-account governance design
* Enterprise RBAC best practices
  
---

## Architecture Flow

```texttext
Employees / Engineers
        ↓
Corporate Identity Provider
(Azure AD / Okta / AD)
        ↓
IAM Identity Center (AWS SSO)
        ↓
Permission Sets
        ↓
Auto-Provisioned IAM Roles
        ↓
AWS Accounts
 ├── Development
 ├── Testing
 ├── Production
 └── Security
```
---

## 🧱 Multi-Account Strategy

### Recommended AWS Organization Structure

```text
AWS Organization
 ├── Security
 ├── Shared Services
 ├── Sandbox
 └── Workloads
      ├── Dev
      ├── Test
      └── Prod
```

## Why Multi-Account?

* Security isolation
* Reduced blast radius
* Separate billing
* Easier governance
* Compliance boundaries
* Safer production environments

---

# 🚀 Step-by-Step Implementation

## STEP 1 — Create AWS Organization

Navigate to:

```text
AWS Organizations
```

Enable:

```text
All Features
```

Create Organizational Units (OUs):

```text
Root
 ├── Security
 ├── Infrastructure
 ├── Sandbox
 └── Workloads
      ├── Dev
      ├── Test
      └── Prod
```

Create accounts:

| Account          | Purpose         |
| ---------------- | --------------- |
| dev-account      | Development     |
| test-account     | QA/UAT          |
| prod-account     | Production      |
| security-account | Logging & Audit |

---

## STEP 2 — Enable IAM Identity Center

Navigate to:

```text
IAM Identity Center
```

Recommended Region:

```text
us-east-1
```

AWS automatically creates:

* Identity Store
* SSO Portal
* Account Assignment Engine
* Role Provisioning Service

Example Portal:

```text
https://company.awsapps.com/start
```

---

## STEP 3 — Configure Identity Provider

### Supported Identity Providers

* Microsoft Entra ID (Azure AD)
* Okta
* Active Directory
* Google Workspace
* Any SAML 2.0 Provider

### Authentication Flow

```text
User Login
   ↓
Identity Provider validates user + MFA
   ↓
SAML Assertion sent to AWS
   ↓
IAM Identity Center authorizes user
   ↓
Temporary credentials issued
```

---

## STEP 4 — Create Users & Groups

### Recommended Groups

| Group          | Purpose             |
| -------------- | ------------------- |
| AWS-Admins     | Full admin access   |
| AWS-Developers | Developer access    |
| AWS-ReadOnly   | Audit access        |
| AWS-Security   | Security operations |
| AWS-FinOps     | Billing access      |

✅ Use group-based access
❌ Avoid assigning permissions directly to users

---

## STEP 5 — Create Permission Sets

Navigate:

```text
IAM Identity Center → Permission Sets
```

### Standard Permission Sets

| Permission Set      | Usage            |
| ------------------- | ---------------- |
| AdministratorAccess | Full admin       |
| PowerUserAccess     | Developer access |
| ReadOnlyAccess      | Audit/view       |
| Billing             | Finance          |

### Internal AWS Flow

```text
Permission Set
    ↓
AWS creates IAM Role
    ↓
User assumes temporary role
```

Example auto-created role:

```text
AWSReservedSSO_AdministratorAccess_xxxxx
```

⚠️ Do NOT manually modify these roles.

---

## STEP 6 — Assign Access

Navigate:

```text
IAM Identity Center → AWS Accounts
```

Assign:

* Group
* Permission Set
* AWS Account

### Example RBAC Matrix

| Group          | Permission    | Account  |
| -------------- | ------------- | -------- |
| AWS-Admins     | Administrator | All      |
| AWS-Developers | PowerUser     | Dev/Test |
| AWS-ReadOnly   | ReadOnly      | Prod     |
| AWS-Security   | ReadOnly      | All      |

---

## STEP 7 — AWS Console Login

Portal URL:

```text
https://company.awsapps.com/start
```

Login Flow:

```text
Login → MFA → Select AWS Account → Open Console
```

AWS automatically issues temporary credentials.

---

## STEP 8 — Configure AWS CLI SSO

### Verify AWS CLI

```bash
aws --version
```

### Configure SSO

```bash
aws configure sso
```

### Login

```bash
aws sso login
```

### Test Access

```bash
aws s3 ls
```

---

## 🔐 Security Best Practices

### Identity Security

* Enforce MFA everywhere
* Avoid root account usage
* Remove long-term access keys
* Use temporary credentials

### Governance

* Use AWS Organizations
* Implement SCPs
* Follow least privilege
* Standardize naming conventions

### Monitoring & Audit

Enable:

* CloudTrail
* AWS Config
* GuardDuty
* Security Hub

Use centralized logging account.

---

## ❌ Common Mistakes

| Mistake                 | Better Approach            |
| ----------------------- | -------------------------- |
| IAM users for employees | Use SSO                    |
| Shared admin accounts   | Use RBAC                   |
| No MFA                  | Enforce MFA                |
| Single AWS account      | Multi-account architecture |
| Long-term access keys   | Temporary credentials      |

---

## 🧠 IAM vs IAM Identity Center

| Feature               | IAM Users | IAM Identity Center |
| --------------------- | --------- | ------------------- |
| Human Access          | ❌         | ✅                   |
| SSO                   | ❌         | ✅                   |
| Temporary Credentials | ❌         | ✅                   |
| Centralized Access    | ❌         | ✅                   |
| MFA Integration       | Limited   | Strong              |
| Multi-Account Access  | Difficult | Easy                |

---

## 🛡 Recommended Enterprise Model

```text
Humans
  ↓
Corporate Identity Provider
  ↓
IAM Identity Center
  ↓
Permission Sets
  ↓
IAM Roles
  ↓
AWS Accounts
  ↓
AWS Resources
```

---

## 📖 Best Places to Learn

### Official AWS Documentation

* AWS IAM Documentation
* IAM Identity Center Documentation
* AWS Organizations Documentation
* AWS Security Best Practices
* AWS Well-Architected Framework

### Hands-On Labs

* AWS Skill Builder
* AWS Workshops
* Cloud Academy
* A Cloud Guru

---

## ⭐ Key Takeaways

### Use IAM Identity Center For

✅ Human users
✅ SSO access
✅ Multi-account access
✅ Temporary credentials

### Use IAM Roles For

✅ EC2
✅ Lambda
✅ ECS Tasks
✅ Cross-account access

### Avoid

❌ IAM users for employees
❌ Shared AWS accounts
❌ Long-term access keys

---

## 🛡 Enterprise Security Summary

```text
Humans → IAM Identity Center + MFA
Workloads → IAM Roles
Governance → Organizations + SCPs
Security → CloudTrail + GuardDuty + Security Hub
```

---

## 🤝 Contributing

Contributions, improvements, and architecture suggestions are welcome.

---
