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

← [[Index]] · [[Home]]
