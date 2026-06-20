---
domain: aws
track: solutions-architect-associate
topic: security
type: note
tags:
  - aws
  - solutions-architect-associate
  - security
  - guardduty
  - inspector
  - macie
  - threat-detection
---

# Threat Detection Services

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/Security#Amazon GuardDuty|Cloud Practitioner: GuardDuty]], [[100 - Cloud/AWS/Cloud Practitioner/Security#Amazon Inspector|Cloud Practitioner: Inspector]], [[100 - Cloud/AWS/Cloud Practitioner/Security#Amazon Macie|Cloud Practitioner: Macie]].

## Amazon GuardDuty

Intelligent threat discovery to protect your AWS account.

**Key features:**
- Uses Machine Learning algorithms, anomaly detection, 3rd party data
- One click to enable (30 days trial), no need to install software
- Can setup EventBridge rules to be notified in case of findings
- EventBridge rules can target AWS Lambda or SNS
- Can protect against CryptoCurrency attacks (has a dedicated finding for it)

### Input Data Sources

| Source                           | What it analyzes                                                                                                                                                |
| -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CloudTrail Events Logs**       | Unusual API calls, unauthorized deployments, suspicious IAM activity, API call patterns from unusual sources                                                    |
| **CloudTrail Management Events** | Infrastructure changes like create VPC subnet, create trail, modify security groups, terminate instances                                                        |
| **CloudTrail S3 Data Events**    | S3 object-level activity: get object, list objects, delete object, unusual access patterns, data exfiltration attempts                                          |
| **VPC Flow Logs**                | Network traffic patterns: unusual internal traffic, unusual IP addresses, port scanning, communication with known malicious IPs                                 |
| **DNS Logs**                     | DNS query patterns: compromised EC2 instances sending encoded data within DNS queries, domain generation algorithms (DGA), communication with malicious domains |

### Optional Features (Additional Data Sources)

- **EKS Audit Logs**: Kubernetes API activity, unauthorized pod creation, suspicious container behavior
- **RDS & Aurora**: Database activity monitoring, unusual query patterns, unauthorized access attempts
- **EBS**: Volume activity, unusual read/write patterns, potential data access anomalies
- **Lambda**: Function invocation patterns, unusual execution behavior, suspicious code execution
- **S3 Malware Protection**: Scan S3 objects for malware as they are uploaded

## Amazon Inspector

Automated security assessments.

### For EC2 Instances

- Leveraging the AWS System Manager (SSM) agent
- Analyze against unintended network accessibility
- Analyze the running OS against known vulnerabilities

### For Container Images (Amazon ECR)

- Assessment of container images as they are pushed

### For Lambda Functions

- Identifies software vulnerabilities in function code and package dependencies
- Assessment of functions as they are deployed

### What Does Amazon Inspector Evaluate?

**Remember:** only for EC2 instances, Container Images & Lambda functions

- Continuous scanning of the infrastructure, only when needed
- **Package vulnerabilities** (EC2, ECR & Lambda) – database of CVE
- **Network reachability** (EC2)
- A risk score is associated with all vulnerabilities for prioritization

### Reporting & Integration

- Integration with AWS Security Hub
- Send findings to Amazon EventBridge

## AWS Macie

Fully managed data security and data privacy service.

**Key features:**
- Uses machine learning and pattern matching
- Discovers and protects sensitive data in AWS
- Helps identify and alert you to sensitive data, such as personally identifiable information (PII)

---

← [[100 - Cloud/AWS/Solutions Architect Associate/Security Services|Security Services]] · [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
