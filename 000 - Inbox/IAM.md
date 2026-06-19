# AWS Organizations
• Global service
• Allows to manage multiple AWS accounts
• The main account is the management account
• Other accounts are member accounts
• Member accounts can only be part of one organization
• Consolidated Billing across all accounts - single payment method
• Pricing benefits from aggregated usage (volume discount for EC2, S3…)
• Shared reserved instances and Savings Plans discounts across accounts
• API is available to automate AWS account creation

• Advantages
• Multi Account vs One Account Multi VPC
• Use tagging standards for billing purposes
• Enable CloudTrail on all accounts, send logs to central S3 account
• Send CloudWatch Logs to central logging account
• Establish Cross Account Roles for Admin purposes
• Security: Service Control Policies (SCP)
• IAM policies applied to OU or Accounts to restrict Users and Roles
• They do not apply to the management account (full admin power)
• Must have an explicit allow from the root through each OU in the direct path to the target account (does not allow anything by default – like IAM)

## Tag Policies
• Helps you standardize tags across resources in an
AWS Organization
• Ensure consistent tags, audit tagged resources,
maintain proper resources categorization, …
• You define tag keys and their allowed values
• Helps with AWS Cost Allocation Tags and
Attribute-based Access Control
• Prevent any non-compliant tagging operations on
specified services and resources (has no effect
on resources without tags)
• Generate a report that lists all tagged/non-
compliant resources
• Use EventBridge to monitor non-compliant tags

# IAM Coditions
aws:SourceIp restrict the client IP from which the API calls are being made

aws:RequestedRegion restrict the region the API calls are made to

ec2:ResourceTag restrict based on tags

aws:MultiFactorAuthPresent to force MFA

## IAM for S3



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
	"Action": [
		"s3:PutObject",
		"s3:GetObject",
		"s3:DeleteObject"
		],
	"Resource": "arn:aws:s3:::test/*"
	}
}

```
• s3:ListBucket permission applies to
arn:aws:s3:::test
• => bucket level permission
• s3:GetObject, s3:PutObject,
s3:DeleteObject applies to
arn:awn:s3:::test/*
• => object level permission

## Resource Policies & aws:PrincipalOrgID
• aws:PrincipalOrgID can be used in any resource policies to restrict
access to accounts that are member of an AWS Organization

## IAM Roles vs Resource Based Policies
• Cross account:
• attaching a resource-based policy to a resource (example: S3 bucket policy)
• OR using a role as a proxy

• When you assume a role (user, application or service), you give up your original
permissions and take the permissions assigned to the role
• When using a resource-based policy, the principal doesn’t have to give up his
permissions
• Example: User in account A needs to scan a DynamoDB table in Account A
and dump it in an S3 bucket in Account B.
• Supported by: Amazon S3 buckets, SNS topics, SQS queues, etc…

• When a rule runs, it needs
 permissions on the target
 • Resource-based policy: Lambda,
 SNS, SQS, S3 buckets, API
 Gateway…
 • IAM role: EC2 Auto Scaling,
 Systems Manager Run
 Command, ECS task…
## IAM Permission Boundaries
• IAM Permission Boundaries are supported for users and roles (not groups)
• Advanced feature to use a managed policy to set the maximum permissions
an IAM entity can get.

• Can be used in combinations of
AWS Organizations SCP

Use cases
• Delegate responsibilities to non
administrators within their permission
boundaries, for example create new IAM
users
• Allow developers to self-assign policies
and manage their own permissions, while
making sure they can’t “escalate” their
privileges (= make themselves admin)
• Useful to restrict one specific user
(instead of a whole account using
Organizations & SCP)


if there is an explicit deny so deny no matter what
if no, then check if there is an allow 

# IAM Identity Center
AWS IAM Identity Center is the AWS solution for connecting your workforce users to AWS managed applications such as Kiro and Amazon Quick, and other AWS resources. You can connect your existing identity provider and synchronize users and groups from your directory, or create and manage your users directly in IAM Identity Center. You can then use IAM Identity Center for either or both of the following:

- User access to applications
    
- User access to AWS accounts
    

**Already using IAM for access to AWS accounts?**

You don’t need to make any changes to your current AWS account workflows to use IAM Identity Center for access to AWS managed applications. If you’re using [federation with IAM](https://docs.aws.amazon.com//IAM/latest/UserGuide/id_roles_providers.html#id_roles_providers_iam) for AWS account access, your users can continue to access AWS accounts in the same way they always have, and you can continue to use your existing workflows to manage that access.

AWS IAM Identity Center
(successor to AWS Single Sign-On)
• One login (single sign-on) for all your
• AWS accounts in AWS Organizations
• Business cloud applications (e.g., Salesforce, Box, Microsoft 365, …)
• SAML2.0-enabled applications
• EC2 Windows Instances
• Identity providers
• Built-in identity store in IAM Identity Center
• 3rd party: Active Directory (AD), OneLogin, Okta…


AWS IAM Identity Center Fine-grained Permissions and Assignments
• Multi-Account Permissions
 • Manage access across AWS accounts in your AWS Organization
 • Permission Sets – a collection of one or more IAM Policies
assigned to users and groups to define AWS access
 • Application Assignments
 • SSO access to many SAML 2.0 business applications (Salesforce,
 Box, Microsoft 365, …)
 • Provide required URLs, certificates, and metadata
 • Attribute-Based Access Control (ABAC)
 • Fine-grained permissions based on users’ attributes stored in
 IAM Identity Center Identity Store
 • Example: cost center, title, locale, …
 • Use case: Define permissions once, then modify AWS access by
 changing the attributes

# AWS Directory Services
• AWS Managed Microsoft AD
	• Create your own AD in AWS, manage users locally, supports MFA
	• Establish “trust” connections with your on-premises AD
• AD Connector
	• Directory Gateway (proxy) to redirect to on-premises AD, supports MFA
	• Users are managed on the on-premises AD
• Simple AD
	• AD-compatible managed directory on AWS
	• Cannot be joined with on-premises AD

# IAM Identity Center – Active Directory Setup
• Connect to an AWS Managed Microsoft AD (Directory Service)
• Integration is out of the box

• Connect to a Self-Managed Directory
	• Create Two-way Trust Relationship using AWS Managed Microsoft AD
	• Create an AD Connector


