---
domain: aws
track: cloud-practitioner
topic: governance
type: note
tags:
  - aws
  - cloud-practitioner
  - governance
  - control-tower
  - service-catalog
  - license-manager
---

# AWS Governance

Services for enforcing rules, managing resources, and controlling software licensing across multi-account AWS environments.

---

## AWS Control Tower

Enforce and manage governance rules for security, operations, and compliance at scale across all organizations and accounts in the AWS Cloud.

**Benefits:** Uses preconfigured controls to quickly set up multi-account environments, automation with built-in governance, and integration of third-party software at scale.

**Use cases:** Quickly deploy applications and provision compliant AWS accounts.

---

## AWS Service Catalog

Create, share, and organize from a curated catalog of AWS resources. Deploy baseline networking resources and security tools for new AWS accounts to govern consistently.

**Benefits:** Saves time by making it quick to find and deploy approved, self-service cloud resources. Improves governance over resources across multiple accounts while staying agile.

**Use cases:** Provision resources across AWS accounts, apply access controls, and accelerate provisioning of CI/CD pipelines.

---

## AWS License Manager

Helps manage software licenses and fine-tune licensing costs. Supports **AWS BYOL (Bring Your Own License)** — use existing licenses purchased from vendors (e.g. Microsoft) on AWS services like EC2 Dedicated Hosts and WorkSpaces.

**Benefits:** Visibility and control over licenses, tracking and managing license usage, and reduced risk of noncompliance.

**Use cases:** Streamline license management, simplify Microsoft License Mobility through Software Assurance, and automate distribution and activation of software entitlements across AWS accounts.

---

## AWS Organizations

Centrally manage and govern your environment as you grow and scale AWS resources. Manage policies for groups of accounts and automate account creation.

**Use cases:** Apply service control policies (SCPs) across accounts, consolidate billing, enforce guardrails at the organizational level. Often used with [[Governance#AWS Control Tower|Control Tower]].

---

## AWS Config

Assess, audit, and evaluate the configurations of your AWS resources. Continuously records configuration changes and evaluates them against desired rules.

**Use cases:** Detect non-compliant resources, track configuration history, trigger remediation actions.

---

## AWS Audit Manager

Continually audits your AWS usage to streamline risk and compliance assessment. Maps AWS usage to compliance frameworks and generates audit-ready reports.

**Use cases:** Simplify audits for standards like PCI DSS, HIPAA, SOC 2.

---

## AWS Artifact

Self-service portal providing on-demand access to AWS security and compliance documentation — reports, certifications, and agreements.

**Use cases:** Download AWS ISO certifications, SOC reports, and sign BAAs (Business Associate Agreements) for HIPAA.

---

← [[README]] · [[Home]]