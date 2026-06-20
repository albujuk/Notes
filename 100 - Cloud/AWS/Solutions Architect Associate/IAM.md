---
domain: aws
track: solutions-architect-associate
topic: security
type: note
tags:
  - aws
  - solutions-architect-associate
  - security
  - iam
  - organizations
  - scp
  - identity-center
  - directory-service
  - control-tower
  - permission-boundaries
  - resource-policies
---

# IAM

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS IAM (Identity and Access Management)|Cloud Practitioner: IAM]] and [[100 - Cloud/AWS/Cloud Practitioner/Governance|Governance]].

## IAM Policy Evaluation Logic

When AWS evaluates whether to allow a request:

1. **Explicit deny** always wins. If any policy says deny, the request is denied, no matter what.
2. If no explicit deny, check for an **explicit allow**. If any policy (identity-based, resource-based, SCP, permissions boundary) allows, the request is allowed.
3. If neither explicit allow nor deny, the default is **implicit deny** (deny).

This applies across all policy types: IAM policies, resource-based policies, SCPs, and permissions boundaries.

## IAM Conditions

Condition keys restrict when a policy statement takes effect:

| Condition Key | Purpose |
|---------------|---------|
| `aws:SourceIp` | Restrict the client IP from which API calls are made |
| `aws:RequestedRegion` | Restrict the region for API calls |
| `ec2:ResourceTag` | Restrict based on EC2 resource tags |
| `aws:MultiFactorAuthPresent` | Force MFA for the action |

## IAM for S3

S3 permissions operate at two levels:

| Level | Example Action | ARN Pattern |
|-------|---------------|-------------|
| **Bucket-level** | `s3:ListBucket` | `arn:aws:s3:::bucket-name` |
| **Object-level** | `s3:GetObject`, `s3:PutObject`, `s3:DeleteObject` | `arn:aws:s3:::bucket-name/*` |

Example policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:ListBucket"],
      "Resource": "arn:aws:s3:::test"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:PutObject", "s3:GetObject", "s3:DeleteObject"],
      "Resource": "arn:aws:s3:::test/*"
    }
  ]
}
```

## Resource-Based Policies vs IAM Roles

### Cross-Account Access Patterns

Two ways to grant cross-account access:

| Method | How It Works | When to Use |
|--------|-------------|-------------|
| **Resource-based policy** | Attach policy directly to the resource (S3 bucket, SNS topic, SQS queue) | Principal keeps their own permissions; supported by S3, SNS, SQS, etc. |
| **IAM role (proxy)** | Assume a role in the target account | Principal gives up original permissions and takes the role's permissions |

**Key difference:** with resource-based policies, the caller doesn't give up their own permissions. With roles, the caller assumes only the role's permissions.

### When Rules Need Permissions

When an event rule runs, it needs permissions on the target:
- **Resource-based policy:** Lambda, SNS, SQS, S3 buckets, API Gateway
- **IAM role:** EC2 Auto Scaling, Systems Manager Run Command, ECS tasks

### aws:PrincipalOrgID

Use `aws:PrincipalOrgID` in any resource policy to restrict access to accounts that are members of a specific AWS Organization.

## IAM Permission Boundaries

Advanced feature: use a managed policy to set the **maximum permissions** an IAM entity (user or role) can get.

**Supported for:** IAM users and IAM roles. **Not supported for:** IAM groups.

**Use cases:**
- Delegate responsibilities to non-administrators within their permission boundaries (e.g. create new IAM users)
- Allow developers to self-assign policies and manage their own permissions, while preventing privilege escalation (can't make themselves admin)
- Restrict one specific user (instead of a whole account using Organizations and SCPs)

Can be used in combination with AWS Organizations SCPs.

## AWS Organizations

Global service for managing multiple AWS accounts.

**Structure:**
- **Management account:** the main (root) account with full admin power
- **Member accounts:** can only be part of one organization
- **Organizational Units (OUs):** group accounts for policy application

**Benefits:**
- Consolidated billing with a single payment method
- Pricing benefits from aggregated usage (volume discounts for EC2, S3, etc.)
- Shared Reserved Instances and Savings Plans discounts across accounts
- API available to automate AWS account creation

**Best practices:**
- Multi-account vs one account with multiple VPCs
- Use tagging standards for billing
- Enable CloudTrail on all accounts, send logs to central S3 account
- Send CloudWatch Logs to central logging account
- Establish cross-account roles for admin purposes

### Service Control Policies (SCPs)

IAM-like policies applied to OUs or accounts to restrict what users and roles can do.

**Key rules:**
- SCPs do **not** apply to the management account (it retains full admin power)
- Must have an explicit allow from the root through each OU in the direct path to the target account
- SCPs do **not** allow anything by default (like IAM: default deny)

### Tag Policies

Standardize tags across resources in an AWS Organization:
- Define tag keys and their allowed values
- Ensure consistent tags, audit tagged resources, maintain proper categorization
- Works with AWS Cost Allocation Tags and Attribute-Based Access Control (ABAC)
- Prevent non-compliant tagging operations on specified services and resources (has no effect on resources without tags)
- Generate reports listing all tagged/non-compliant resources
- Use EventBridge to monitor non-compliant tags

## AWS Control Tower

Easy way to set up and govern a secure, compliant multi-account AWS environment based on best practices. Uses AWS Organizations to create accounts.

**Benefits:**
- Automate environment setup in a few clicks
- Automate ongoing policy management using guardrails
- Detect policy violations and remediate them
- Monitor compliance through an interactive dashboard

### Guardrails

Provide ongoing governance for the Control Tower environment:

| Type | Mechanism | Example |
|------|-----------|---------|
| **Preventive** | SCPs | Restrict regions across all accounts |
| **Detective** | AWS Config | Identify untagged resources |

## IAM Identity Center

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS IAM Identity Center|Cloud Practitioner: IAM Identity Center]].

Successor to AWS Single Sign-On. One login for:
- All AWS accounts in AWS Organizations
- Business cloud applications (Salesforce, Box, Microsoft 364, etc.)
- SAML 2.0-enabled applications
- EC2 Windows instances

### Identity Providers

- Built-in identity store in IAM Identity Center
- Third-party: Active Directory (AD), OneLogin, Okta, etc.

### Permission Sets and Assignments

**Multi-Account Permissions:**
- Manage access across AWS accounts in your AWS Organization
- **Permission Sets:** a collection of one or more IAM policies assigned to users and groups to define AWS access

**Application Assignments:**
- SSO access to many SAML 2.0 business applications (Salesforce, Box, Microsoft 364, etc.)
- Provide required URLs, certificates, and metadata

**Attribute-Based Access Control (ABAC):**
- Fine-grained permissions based on users' attributes stored in IAM Identity Center Identity Store
- Attributes: cost center, title, locale, etc.
- Use case: define permissions once, then modify AWS access by changing attributes

## AWS Directory Services

| Type | Description |
|------|-------------|
| **AWS Managed Microsoft AD** | Create your own AD in AWS, manage users locally, supports MFA. Establish "trust" connections with on-premises AD |
| **AD Connector** | Directory gateway (proxy) to redirect to on-premises AD, supports MFA. Users are managed on the on-premises AD |
| **Simple AD** | AD-compatible managed directory on AWS. Cannot be joined with on-premises AD |

### IAM Identity Center and Active Directory

- **AWS Managed Microsoft AD:** integration is out of the box
- **Self-managed directory:** two options:
  - Create a Two-way Trust Relationship using AWS Managed Microsoft AD
  - Create an AD Connector

---

← [[100 - Cloud/AWS/Solutions Architect Associate/KMS|KMS]] · [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
