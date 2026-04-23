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
---

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

← [[Index]] · [[Home]]
