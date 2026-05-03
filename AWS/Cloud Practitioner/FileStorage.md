---
domain: aws
track: cloud-practitioner
topic: file-storage
type: note
tags:
  - aws
  - cloud-practitioner
  - storage
  - file-storage
  - efs
  - fsx
  - nfs
---
# File Storage

## Amazon Elastic File System (EFS)

Fully managed, scalable **file storage** using the **Linux NFS protocol**. Auto-scales to petabytes as files are added or removed — no disruption to applications. Multiple EC2 instances can access the same EFS filesystem simultaneously (unlike [[BlockStorage#Amazon Elastic Block Store (EBS)|EBS]], which attaches to one instance).

Use cases: shared file systems, content repositories, web serving, home directories, big data analytics.

### EFS Storage Classes

| Storage Class | Use Case |
|---------------|----------|
| **EFS Standard** | Frequently accessed files |
| **EFS Standard-IA** | Infrequent access, lower cost |
| **EFS One Zone** | Single AZ, frequent access |
| **EFS One Zone-IA** | Single AZ, infrequent access, lowest cost |

No minimum fee or setup cost — pay only for storage used.

---

## Amazon FSx

Fully managed service to launch, run, and scale **high-performance file systems** in the cloud. Supports multiple filesystem protocols. Handles hardware provisioning, patching, and backups.

Compared to [[FileStorage#Amazon Elastic File System (EFS)|EFS]] (NFS-only), FSx supports a broader set of protocols and workload types.

### FSx Variants

| Variant | Protocol | Key Use Cases |
|---------|----------|---------------|
| **FSx for Windows File Server** | SMB / Windows Server | Migrate Windows file servers, hybrid workloads, SQL Server, virtual desktops |
| **FSx for NetApp ONTAP** | Multi-protocol (NFS, SMB, iSCSI) | Seamless migration, modern apps, data management, business continuity |
| **FSx for OpenZFS** | NFS (v3, v4, v4.1, v4.2) | Migrate workloads, data analytics, content management, dev/test |
| **FSx for Lustre** | Lustre (parallel) | ML, HPC, big data analytics, media processing |

---

← [[Storage]] · [[README]] · [[Home]]
