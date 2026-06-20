---
domain: aws
track: solutions-architect-associate
topic: compute
type: note
tags:
  - aws
  - solutions-architect-associate
  - compute
  - ec2
  - ami
  - amazon-machine-image
---

# AMI: Amazon Machine Image

Customization of EC2 instances that reduces boot and setup time by pre-baking configurations.

AMI is built for a **specific region** and can be copied to other regions.

> For a broader overview of AMIs and their role in EC2, see [[100 - Cloud/AWS/Cloud Practitioner/Compute#AMI: Amazon Machine Image|Cloud Practitioner: AMI]].

## AMI Types

| Type | Source | Use case |
|------|--------|----------|
| **Public (by AWS)** | Amazon-provided | Amazon Linux, Ubuntu, Windows Server, etc. |
| **Private (by you)** | Custom, from your own instance | Your own pre-configured templates |
| **AWS Marketplace** | Third-party vendors | Commercial or free pre-built software stacks |

## AMI Sharing Process (Encrypted via KMS)

When sharing an AMI encrypted with KMS across accounts:

1. AMI in Source Account is encrypted with KMS Key from Source Account
2. Must modify the image attribute to add a Launch Permission which corresponds to the specified target AWS account
3. Must share the KMS Keys used to encrypt the snapshot the AMI references with the target account / IAM Role
4. The IAM Role/User in the target account must have the permissions to DescribeKey, ReEncrypt*, CreateGrant, Decrypt
5. When launching an EC2 instance from the AMI, optionally the target account can specify a new KMS key in its own account to re-encrypt the volumes

---

← [[100 - Cloud/AWS/Solutions Architect Associate/EC2|EC2]] · [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
