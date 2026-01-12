# AWS IAM (Identity and Access Management)

Complete guide to AWS IAM - the foundation of security and access control in AWS.

---

## 📋 Table of Contents

- [What is IAM?](#-what-is-iam)
- [IAM Components Deep Dive](#-iam-components-deep-dive)
- [Root Account vs IAM User](#-root-account-vs-iam-user)
- [IAM Identity Center](#-iam-identity-center)
- [Principle of Least Privilege](#-principle-of-least-privilege)
- [IAM Best Practices](#-iam-best-practices)

---

## 🔑 What is IAM?

### Definition

**IAM = Identity and Access Management**

A web service that helps you securely control access to AWS resources by managing authentication and authorization.
```
┌─────────────────────────────────────┐
│   IAM Controls Access To AWS        │
├─────────────────────────────────────┤
│ ✓ Centralized access control        │
│ ✓ Manage WHO can do WHAT            │
│ ✓ Authentication + Authorization    │
└─────────────────────────────────────┘
```

### Core Functions

| Function | Description |
| :--- | :--- |
| 🔐 **Authentication** | "Who are you?" - Verifying identity |
| ✅ **Authorization** | "What can you do?" - Granting permissions |

### Key Concepts

| Component | Purpose |
| :--- | :--- |
| **Users** | People or applications that need AWS access |
| **Groups** | Collection of users with similar permissions |
| **Roles** | Temporary credentials for AWS services/resources |
| **Policies** | JSON documents defining permissions |
| **Root Account** | The email used to create AWS account (⚠️ Never use for daily tasks) |

---

## 🔍 IAM Components Deep Dive

### A. IAM Users
```
┌─────────────────────────────────────┐
│          IAM Users                   │
├─────────────────────────────────────┤
│ Who: Individual people or services   │
│ When: Long-term access needed        │
│ Best Practice: One user per person   │
└─────────────────────────────────────┘
```

#### Characteristics

| Feature | Description |
| :--- | :--- |
| **Credentials** | Permanent (Access Key ID + Secret Access Key) |
| **Console Access** | Can have password for AWS Management Console login |
| **Permission Assignment** | Directly or via groups |
| **Long-term** | Credentials don't expire automatically |

#### Example Use Cases

- Developer accessing AWS Console
- CI/CD pipeline with programmatic access
- Database administrator managing RDS

---

### B. IAM Groups
```
┌─────────────────────────────────────┐
│          IAM Groups                  │
├─────────────────────────────────────┤
│ Purpose: Organize similar users      │
│ Example: Developers, Admins          │
│ Rule: Users → Multiple groups ✓      │
└─────────────────────────────────────┘
```

#### Common Group Structure
```
Organization
├── Administrators (Full access)
├── Developers (EC2, S3, RDS access)
├── DataAnalysts (Read-only S3, Athena)
└── Auditors (CloudTrail, CloudWatch read)
```

#### Key Benefits

- 📦 Simplified permission management
- 🔄 Easy user onboarding/offboarding
- 📊 Clear organizational structure
- ⚡ Bulk permission updates

> 💡 **Note**: A user can belong to multiple groups and inherits permissions from all groups.

---

### C. IAM Roles
```
┌─────────────────────────────────────┐
│          IAM Roles                   │
├─────────────────────────────────────┤
│ Who: AWS services or applications    │
│ When: Temporary access needed        │
│ Key: No credentials stored           │
└─────────────────────────────────────┘
```

#### Common Use Cases

| Scenario | Example |
| :--- | :--- |
| **Service-to-Service** | EC2 instance accessing S3 buckets |
| **Serverless** | Lambda function reading DynamoDB tables |
| **Cross-Account** | Account A accessing resources in Account B |
| **Federation** | External users accessing AWS via SAML/OIDC |
| **Emergency Access** | Temporary elevated privileges |

#### Role Flow Diagram
```
1. Service assumes role
   ↓
2. AWS STS provides temporary credentials
   ↓
3. Service uses credentials (valid 15min - 12hrs)
   ↓
4. Credentials expire automatically
```

---

### D. IAM Policies

Policies are JSON documents that define permissions.

#### Policy Structure
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowS3ReadAccess",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::my-bucket",
                "arn:aws:s3:::my-bucket/*"
            ],
            "Condition": {
                "IpAddress": {
                    "aws:SourceIp": "192.168.1.0/24"
                }
            }
        }
    ]
}
```

#### Policy Elements Explained

| Element | Required | Description |
| :--- | :---: | :--- |
| **Version** | ✅ | Policy language version (always `2012-10-17`) |
| **Statement** | ✅ | Array of permission statements |
| **Effect** | ✅ | `Allow` or `Deny` |
| **Action** | ✅ | List of AWS service actions |
| **Resource** | ✅ | ARN of resources affected |
| **Condition** | ❌ | Optional conditions (IP, time, MFA, etc.) |

#### Policy Types

| Type | Description | Use Case |
| :--- | :--- | :--- |
| **AWS Managed** | Created and maintained by AWS | Quick start, common scenarios |
| **Customer Managed** | Created by you | Custom business requirements |
| **Inline** | Directly attached to single entity | One-off, specific permissions |
| **Identity-based** | Attached to users/groups/roles | Control what identity can do |
| **Resource-based** | Attached to resources (S3, SQS) | Control who can access resource |

#### Example Policy Scenarios

**Read-Only S3 Access**
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": "*"
        }
    ]
}
```

**Deny All Actions Outside Business Hours**
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "*",
            "Condition": {
                "DateGreaterThan": {"aws:CurrentTime": "2024-01-01T17:00:00Z"},
                "DateLessThan": {"aws:CurrentTime": "2024-01-01T09:00:00Z"}
            }
        }
    ]
}
```

---

## ⚠️ Root Account vs IAM User

### Critical Comparison

| Feature | Root Account | IAM User |
| :--- | :---: | :---: |
| **Creation** | Automatic (AWS signup) | Manual creation |
| **Permissions** | Unlimited (cannot restrict) | Limited by policies |
| **Login Method** | Email + Password | Username + Password |
| **MFA** | Optional (but critical) | Optional |
| **Use Case** | ❌ Never for daily tasks | ✅ All daily operations |
| **Restrictions** | None | Policy-controlled |
| **Audit Trail** | Limited | Full CloudTrail logging |

### Root Account Dangers
```
🚨 ROOT ACCOUNT RISKS
├── No permission boundaries
├── Cannot be restricted by policies
├── If compromised → ENTIRE account at risk
├── Limited audit trail visibility
└── Can delete everything including logs
```

### Root Account - When to Use

Only use root account for these specific tasks:

1. ✅ Change account settings (email, password, MFA)
2. ✅ Close AWS account
3. ✅ Restore IAM user permissions (if locked out)
4. ✅ Change or cancel AWS Support plan
5. ✅ Register as a seller in Reserved Instance Marketplace
6. ✅ Configure S3 bucket for MFA Delete
7. ✅ Edit/delete S3 bucket policy with invalid VPC/VPC endpoint ID

### Securing Root Account
```yaml
Essential Steps:
  1. Enable MFA immediately (hardware token preferred)
  2. Delete access keys (if any exist)
  3. Use strong, unique password (20+ characters)
  4. Never share credentials
  5. Store credentials in secure vault (not email/notes)
  6. Create IAM admin user for daily tasks
  7. Lock away root credentials
```

---

## 🌐 IAM Identity Center

Formerly known as **AWS Single Sign-On (SSO)**.

### Overview
```
┌─────────────────────────────────────┐
│    IAM Identity Center               │
├─────────────────────────────────────┤
│ Purpose: Centralized access to       │
│          multiple AWS accounts       │
└─────────────────────────────────────┘
```

### Key Features

| Feature | Benefit |
| :--- | :--- |
| 🔐 **Single Sign-On** | One login for all AWS accounts and applications |
| 👥 **Multi-Account Management** | Manage access across AWS Organizations |
| 🔄 **User Provisioning** | Sync from Active Directory/Okta/Azure AD |
| ⏱️ **Temporary Credentials** | No long-term access keys |
| 📱 **Application Integration** | SSO for third-party SaaS apps |

### Use Cases

- **Enterprise Organizations**: 100+ AWS accounts
- **Active Directory Integration**: Existing on-prem identity
- **Compliance Requirements**: Centralized audit logs
- **Developer Portal**: Self-service access requests

### Architecture Example
```
External Identity Provider (Okta/Azure AD)
            ↓
    IAM Identity Center
            ↓
    ┌───────────────────────┐
    │   AWS Organization    │
    ├───────────────────────┤
    │ • Production Account  │
    │ • Development Account │
    │ • Testing Account     │
    │ • Security Account    │
    └───────────────────────┘
```

---

## 🔒 Principle of Least Privilege

### Definition

> **Grant only the minimum permissions necessary to perform a specific task.**

### Why It Matters

| Benefit | Impact |
| :--- | :--- |
| 🛡️ **Security** | Limits blast radius of compromised credentials |
| 📉 **Risk Reduction** | Minimizes accidental/malicious damage |
| ✅ **Compliance** | Meets regulatory requirements (SOC2, ISO 27001) |
| 🔍 **Auditability** | Clear permission boundaries |

### Implementation Steps
```
Step 1: Start with ZERO permissions
   ↓
Step 2: Grant minimum needed for task
   ↓
Step 3: Test functionality
   ↓
Step 4: Monitor access patterns (CloudTrail)
   ↓
Step 5: Add permissions only when justified
   ↓
Step 6: Regular reviews (quarterly)
   ↓
Step 7: Remove unused permissions
```

### Practical Example

**❌ Bad Practice: Over-Permissive**
```json
{
    "Effect": "Allow",
    "Action": "s3:*",
    "Resource": "*"
}
```

**✅ Good Practice: Least Privilege**
```json
{
    "Effect": "Allow",
    "Action": [
        "s3:GetObject",
        "s3:PutObject"
    ],
    "Resource": "arn:aws:s3:::specific-bucket/logs/*"
}
```

### Tools to Enforce Least Privilege

| Tool | Purpose |
| :--- | :--- |
| **IAM Access Analyzer** | Identifies over-permissive policies |
| **IAM Policy Simulator** | Test policies before deployment |
| **CloudTrail Insights** | Detect unusual access patterns |
| **AWS Config** | Monitor policy compliance |

---

## ✅ IAM Best Practices

### Security Checklist
```yaml
Essential Practices:

1. MFA Enforcement:
   - Enable MFA for root account (hardware token)
   - Enable MFA for all IAM users with console access
   - Consider MFA for CLI/API access (critical accounts)

2. Group-Based Permissions:
   - Never attach policies directly to users
   - Use groups for all permission assignments
   - Create groups by job function

3. Password Policy:
   - Minimum 14 characters
   - Require uppercase, lowercase, numbers, symbols
   - 90-day rotation policy
   - Prevent password reuse (last 24 passwords)

4. Roles Over Users:
   - Use roles for EC2/Lambda/ECS applications
   - Never embed access keys in code
   - Prefer temporary credentials

5. Credential Rotation:
   - Rotate access keys every 90 days
   - Rotate passwords every 90 days
   - Use AWS Secrets Manager for automation

6. Monitoring & Auditing:
   - Enable CloudTrail in all regions
   - Send logs to S3 with versioning
   - Set up CloudWatch alarms for:
     • Root account usage
     • Failed login attempts
     • Policy changes
     • Access key creation

7. Cleanup:
   - Delete unused IAM users
   - Remove inactive credentials (>90 days)
   - Remove overly broad permissions
   - Archive old access keys (don't delete immediately)

8. Policy Conditions:
   - Restrict by IP address (office/VPN)
   - Require MFA for sensitive actions
   - Time-based restrictions (business hours)
   - Enforce SSL/TLS for API calls
```

### Advanced Best Practices

| Practice | Implementation |
| :--- | :--- |
| **Service Control Policies (SCPs)** | Organization-wide permission boundaries |
| **Permission Boundaries** | Maximum permissions for IAM entities |
| **Resource Tags** | Attribute-based access control (ABAC) |
| **Cross-Account Roles** | Secure access between accounts |
| **IAM Conditions** | Context-based access (IP, time, MFA) |

### Security Assessment Checklist

Use this to audit your IAM setup:

- [ ] Root account MFA enabled
- [ ] Root account access keys deleted
- [ ] No users with `AdministratorAccess` policy
- [ ] All users in groups (no direct policy attachments)
- [ ] Password policy configured
- [ ] MFA enabled for privileged users
- [ ] CloudTrail enabled and logging
- [ ] IAM Access Analyzer enabled
- [ ] Regular access review process (quarterly)
- [ ] Unused credentials removed
- [ ] Service roles used for applications
- [ ] No access keys in code repositories

---

## 🎯 Quick Reference

### Common IAM Actions
```bash
# Create IAM user
aws iam create-user --user-name john-doe

# Create group
aws iam create-group --group-name developers

# Add user to group
aws iam add-user-to-group --user-name john-doe --group-name developers

# Attach policy to group
aws iam attach-group-policy --group-name developers \
    --policy-arn arn:aws:iam::aws:policy/PowerUserAccess

# List users
aws iam list-users

# Create access key
aws iam create-access-key --user-name john-doe

# Enable MFA
aws iam enable-mfa-device --user-name john-doe \
    --serial-number arn:aws:iam::123456789012:mfa/john-doe \
    --authentication-code-1 123456 --authentication-code-2 789012
```

---

## 📚 Additional Resources

- [AWS IAM Documentation](https://docs.aws.amazon.com/iam/)
- [IAM Policy Simulator](https://policysim.aws.amazon.com/)
- [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [IAM Access Analyzer](https://aws.amazon.com/iam/access-analyzer/)
- [AWS Security Blog](https://aws.amazon.com/blogs/security/)

---

## 🔗 Related Topics

- **Next**: [AWS Global Infrastructure](../infrastructure/README.md)
- **Related**: [AWS Organizations](../organizations/README.md)
- **Advanced**: [AWS Security Best Practices](../security/README.md)

