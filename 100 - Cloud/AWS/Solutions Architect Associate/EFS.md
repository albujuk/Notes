---
domain: aws
track: solutions-architect-associate
topic: storage
type: note
tags:
  - aws
  - solutions-architect-associate
  - storage
  - efs
  - nfs
---

# EFS: Elastic File System

Managed NFS (Network File System). Can be mounted across AZs and regions.

Highly available, scalable, expensive (3x gp2), pay-per-use with no pre-provisioning required.

- Uses **NFSv4.1** protocol
- Uses **security groups** to control access
- Compatible with **Linux-based AMIs only** (not Windows)
- Encryption at rest using **KMS**
- POSIX file system with standard file API
- Scales automatically, no capacity planning needed

**Use cases:** content management, web serving, data sharing, WordPress

> For foundational EFS concepts, see [[100 - Cloud/AWS/Cloud Practitioner/FileStorage#Amazon Elastic File System (EFS)|Cloud Practitioner: EFS]].

## Scale

- 1000s of concurrent NFS clients/connections
- 10 GB+/s throughput
- Grows automatically as usage increases

## Performance Mode

Set at EFS creation time:

1. **General Purpose** (default): latency-sensitive use cases
2. **Max I/O**: higher latency, higher throughput, highly parallel (big data workloads)

## Throughput Mode

1. **Bursting**: 1 TB = 50 MiB/s + burst up to 100 MiB/s
2. **Provisioned**: set throughput independently of storage size (e.g. 1 GiB/s for 1 TB storage)
3. **Elastic**: automatically scales throughput up or down based on workload; used for unpredictable workloads

## Storage Classes

| Class | Access pattern |
|-------|----------------|
| **Standard** | Frequently accessed data |
| **Infrequent Access (IA)** | Accessed a few times per quarter |
| **Archive** | Accessed a few times per year or less |

### Lifecycle Transitions (Default)

- Files not accessed in Standard for **30 days** → moved to IA
- Files not accessed in Standard for **90 days** → moved to Archive
- Files are **not** moved back to Standard on access

For performance-sensitive workloads (many small files), set transition to Standard **on first access**.

Using correct storage classes can reduce cost up to **90%**.

## Deployment

- **Multi-AZ EFS**: use for production
- **Single-AZ EFS**: use for dev/test; compatible with IA

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]