---
domain: aws
track: cloud-practitioner
topic: security
type: note
tags:
  - aws
  - cloud-practitioner
  - security
  - shared-responsibility
  - iam
  - shield
  - waf
  - kms
  - encryption
  - macie
  - inspector
  - guardduty
  - detective
  - security-hub
---
d
# AWS Security

AWS offers multiple security mechanisms to protect cloud resources across three goals:

- **Prevent** — proper permission and access management
- **Protect** — networks, applications, and data
- **Detect & respond** — identify and react to incidents as they occur

---

## AWS Shared Responsibility Model

Cloud security is a shared responsibility between AWS and the customer.

**Customers: Security _in_ the cloud**

Customers maintain complete control over their content and are responsible for securing everything they create and manage in AWS:

- Managing security of data, systems, and applications
- Deciding what data and workloads to store or run in AWS
- Determining which AWS services to use
- Controlling who has access to environments and resources

**AWS: Security _of_ the cloud**

AWS is responsible for securing the infrastructure that runs AWS services:

- The foundational software powering AWS services
- The virtualization layer
- The hardware and global infrastructure (data centers, [[Global Infrastructure#AWS Regions|Regions]], [[Global Infrastructure#Availability Zones|Availability Zones]], [[Global Infrastructure#Edge Locations|edge locations]])

---

---

## AWS IAM (Identity and Access Management)

**Securely manage identities and access to AWS services and resources.**

By default, all actions in AWS are denied. Permissions must be explicitly granted.

> **Principle of least privilege** — give people and systems access only to what they need and nothing else.

IAM provides **users**, **groups**, and **roles** to configure access based on operational and security needs. **IAM policies** define the permissions for these identities.

### IAM Policies

**JSON documents** that define _what actions are allowed or denied_ on _which resources_. Think of them as the rulebook.

### IAM Roles

A role is an **identity without permanent credentials** — it's assumed temporarily by a user, service, or application. For example, an EC2 instance assumes a role to access S3, rather than having hardcoded credentials. Roles have policies attached to them that define what the role can do.

By default, IAM role session credentials are valid for **1 hour**, but the maximum session duration can be configured to up to **12 hours** (43,200 seconds).

### Access Keys

**Long-term credentials** (Access Key ID + Secret Access Key) used to authenticate programmatically via CLI, SDK, or API calls. They prove _who you are_, not _what you can do_.

- Permissions are still determined by the **policies** attached to that user
- Associated with **IAM users**, not roles
- Considered less secure than roles because they're static and can leak

### Mental Model

| Concept        | Answers the question...        | Example                                 |
| -------------- | ------------------------------ | --------------------------------------- |
| **Policy**     | What am I _allowed_ to do?     | Allow `s3:PutObject` on `my-bucket`     |
| **Role**       | _Who/what_ am I (temporarily)? | EC2 instance acting as a backup service |
| **Access Key** | How do I _prove_ who I am?     | CLI credentials for a CI/CD pipeline    |

---

## Additional Access Management Services

### AWS IAM Identity Center

Centralizes identity and access management across AWS accounts and applications. Connects to an existing identity source and provides workforce **single sign-on (SSO)** access to all connected AWS services and accounts.

> **Federated identity management** — system that allows users to access multiple applications/services/domains using a single set of credentials.

### AWS Secrets Manager

Securely manages, rotates, and retrieves database credentials, API keys, and other secrets throughout their lifecycle.

> **Secrets** — confidential information intended for specific individuals/groups (e.g. passwords, DB credentials, API keys).

### AWS Systems Manager

Provides centralized view of nodes across accounts, Regions, multi-cloud, and hybrid environments. Enables quick access to node info and automation of registry edits, user management, and security patching.

> **Nodes** — connection points in a network, system, or structure.

---

## Network & Application Protection

| Service | Description |
|---|---|
| **AWS Shield** | Protects against DDoS attacks. Standard is free and automatic; Advanced adds enhanced detection and 24/7 support. |
| **AWS WAF** | Filters web traffic using a web ACL — allows or blocks requests by IP, header, URI, or geo. |

### AWS Shield

Two tiers: **Standard** (automatic, free, covers common DDoS vectors) and **Advanced** (enhanced detection, real-time metrics, cost protection, 24/7 DRT support).

> **DDoS** — attack that floods a system with traffic to make it unavailable to legitimate users.

### AWS WAF (Web Application Firewall)

Filters incoming web traffic using a **web ACL** that defines rules to allow or block requests based on IP addresses, HTTP headers, URI strings, or geographic location.

> **Web ACL** — ordered set of rules WAF evaluates against each incoming request.

---

## Data Protection

| Service | Description |
|---|---|
| **AWS KMS** | Creates and manages cryptographic keys to encrypt/decrypt data across AWS services. |
| **AWS CloudHSM** | Dedicated single-tenant hardware security module for key management and cryptographic operations. |
| **Amazon Macie** | Uses ML to discover and protect sensitive data (PII, credentials) in [[S3]]. |
| **AWS Certificate Manager (ACM)** | Provisions and auto-renews SSL/TLS certificates for encrypting data in transit. |

### AWS KMS (Key Management Service)

Creates and manages **cryptographic keys** used to encrypt and decrypt data across AWS services. Provides centralized key control with full audit logging via CloudTrail.

### AWS CloudHSM

Dedicated **hardware security module (HSM)** — single-tenant, fully managed. Use when compliance requirements mandate exclusive hardware control over key management.

### Amazon Macie

Uses machine learning to automatically discover, classify, and protect **sensitive data** (PII, credentials) stored in [[S3]]. Generates findings when sensitive data is exposed or at risk.

### AWS Certificate Manager (ACM)

Provisions and manages **SSL/TLS certificates** for use with AWS services. Handles automatic renewal, ensuring data is encrypted in transit.

> **SSL/TLS** — protocols that encrypt data between clients and servers (HTTPS).

### Encryption Services — Quick Comparison

| Service | Primary Purpose |
|---------|----------------|
| **AWS KMS** | Create and manage encryption keys for data at rest across AWS services |
| **AWS CloudHSM** | Dedicated hardware for key management (single-tenant) |
| **AWS Certificate Manager** | SSL/TLS certificates for data in transit |
| **AWS Secrets Manager** | Store and rotate secrets/credentials |

---

## Detection & Response

| Service | Description |
|---|---|
| **Amazon Inspector** | Scans EC2, container images, and Lambda for software vulnerabilities and network exposure. |
| **Amazon GuardDuty** | Continuously monitors for malicious activity via CloudTrail, VPC Flow Logs, and DNS — no agents needed. |
| **Amazon Detective** | Investigates security incidents using interactive visualizations built from log data. |
| **AWS Security Hub** | Aggregates findings from GuardDuty, Inspector, Macie, and third-party tools into a single prioritized dashboard. |

### Amazon Inspector

Automatically scans EC2 instances, container images, and Lambda functions for **software vulnerabilities** and unintended network exposure. Produces a risk score to prioritize findings.

### Amazon GuardDuty

Continuously monitors AWS accounts and workloads for **malicious activity** using threat intelligence, ML, and behavioral analysis. Analyzes CloudTrail, VPC Flow Logs, and DNS logs — no agents required.

### Amazon Detective

Helps **investigate security incidents** by building interactive visualizations from log data. Surfaces relationships between resources, IPs, and user activity to speed root-cause analysis.

### AWS Security Hub

**Aggregates findings** from GuardDuty, Inspector, Macie, and third-party tools into a single dashboard. Normalizes and prioritizes findings into actionable insights using security standards (e.g. CIS benchmarks).

---

## IAM Access Analyzer

Provides capabilities to set, verify, and refine security permissions to achieve **least privilege** security standards. Analyzes resource policies to identify external access and validate that permissions match intent.

**Use cases:** Find overly permissive policies, validate policies before deploying, generate least-privilege policies from access activity.

---

→ [docs.aws.amazon.com — Security](https://docs.aws.amazon.com/security/)

← [[100 - Cloud/AWS/Cloud Practitioner/README|Cloud Practitioner]]
