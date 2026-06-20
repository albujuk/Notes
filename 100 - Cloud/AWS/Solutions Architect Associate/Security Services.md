---
domain: aws
track: solutions-architect-associate
topic: security
type: note
tags:
  - aws
  - solutions-architect-associate
  - security
  - ssm
  - parameter-store
  - secrets-manager
  - acm
  - certificates
---

# Security Services

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/Security|Cloud Practitioner: Security]].

## SSM Parameter Store

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Systems Manager|Cloud Practitioner: Systems Manager]].

Secure storage for configuration and secrets.

**Features:**
- Optional seamless encryption using KMS
- Serverless, scalable, durable, easy SDK
- Version tracking of configurations / secrets
- Security through IAM
- Notifications with Amazon EventBridge
- Integration with CloudFormation

### Standard vs Advanced Parameters

| | Standard | Advanced |
|---|----------|----------|
| **Total number of parameters allowed** (per AWS account and Region) | 10,000 | 100,000 |
| **Maximum size of a parameter value** | 4 KB | 8 KB |
| **Parameter policies available** | No | Yes |
| **Cost** | No additional charge | Charges apply |
| **Storage Pricing** | Free | $0.05 per advanced parameter per month |

### Parameter Policies (Advanced Parameters)

- Allow to assign a TTL to a parameter (expiration date) to force updating or deleting sensitive data such as passwords
- Can assign multiple policies at a time

## AWS Secrets Manager

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Secrets Manager|Cloud Practitioner: Secrets Manager]].

Newer service, meant for storing secrets.

**Features:**
- Capability to force rotation of secrets every X days
- Automate generation of secrets on rotation (uses Lambda)
- Integration with Amazon RDS (MySQL, PostgreSQL, Aurora)
- Secrets are encrypted using KMS
- Mostly meant for RDS integration

### Multi-Region Secrets

- Replicate Secrets across multiple AWS Regions
- Secrets Manager keeps read replicas in sync with the primary Secret
- Ability to promote a read replica Secret to a standalone Secret
- **Use cases:** multi-region apps, disaster recovery strategies, multi-region DB

## AWS Certificate Manager (ACM)

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Certificate Manager (ACM)|Cloud Practitioner: ACM]].

Easily provision, manage, and deploy TLS Certificates.

**Features:**
- Provide in-flight encryption for websites (HTTPS)
- Supports both public and private TLS certificates
- Free of charge for public TLS certificates
- Automatic TLS certificate renewal

**Integrations (load TLS certificates on):**
- Elastic Load Balancers (CLB, ALB, NLB)
- CloudFront Distributions
- APIs on API Gateway
- **Cannot use ACM with EC2** (can't be extracted)

### Requesting Public Certificates

1. List domain names to be included in the certificate:
   - Fully Qualified Domain Name (FQDN): corp.example.com
   - Wildcard Domain: *.example.com
2. Select Validation Method: DNS Validation or Email validation
   - **DNS Validation is preferred** for automation purposes
   - Email validation will send emails to contact addresses in the WHOIS database
   - DNS Validation will leverage a CNAME record to DNS config (ex: Route 53)
3. It will take a few hours to get verified
4. The Public Certificate will be enrolled for automatic renewal
   - ACM automatically renews ACM-generated certificates 60 days before expiry

### Importing Public Certificates

- Option to generate the certificate outside of ACM and then import it
- **No automatic renewal**, must import a new certificate before expiry
- ACM sends daily expiration events starting 45 days prior to expiration
  - The # of days can be configured
  - Events are appearing in EventBridge
- AWS Config has a managed rule named `acm-certificate-expiration-check` to check for expiring certificates (configurable number of days)

---

← [[100 - Cloud/AWS/Solutions Architect Associate/KMS|KMS]] · [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
