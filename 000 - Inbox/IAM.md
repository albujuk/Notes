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
