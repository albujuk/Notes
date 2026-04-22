---
domain: aws
track: cloud-practitioner
topic: storage
type: note
tags:
  - aws
  - cloud-practitioner
  - storage
  - ebs
  - ec2
  - instance-store
  - block-storage
  - ebs-snapshots
  - data-lifecycle-manager
  - s3
  - object-storage
  - s3-storage-classes
  - glacier
---
# Storage

## EC2 Instance Store

Block-level storage **physically attached** to the [[Compute#EC2 — Elastic Compute Cloud|EC2]] host computer. Not a standalone AWS service — bundled with certain instance types.

**Key property: no persistence.** Data is lost when the instance is stopped or terminated.

Best for: buffers, caches, scratch data, temporary computation. Not recommended for applications that require data retention.

## Amazon Elastic Block Store (EBS)

Persistent block-level storage volumes that attach to [[Compute#EC2 — Elastic Compute Cloud|EC2 instances]] like external hard drives. Survives instance stops and terminations — data remains available regardless of instance state.

**Key property: persistence.** Stop or terminate the instance; EBS data stays.

Back up EBS volumes with **EBS snapshots** (incremental, stored in S3).

### Use cases

| Use case                | Notes                                |
| ----------------------- | ------------------------------------ |
| Database hosting        | Low-latency, consistent IOPS         |
| App backup storage      | Snapshots for point-in-time recovery |
| Dev environment cloning | Launch a snapshot as a new volume    |

### Benefits

| Capability                | How it helps                                              |
| ------------------------- | --------------------------------------------------------- |
| **Data migration**        | Snapshot → restore in another AZ or region                |
| **Instance type changes** | Detach volume, attach to new instance type — no data loss |
| **Disaster recovery**     | Snapshots restorable in different regions                 |
| **Cost optimization**     | Modify volume type or size without downtime               |
| **Performance tuning**    | Switch volume type (gp3, io2, etc.) on the fly            |

### EBS Snapshots

Point-in-time backups of an EBS volume. Stored redundantly across multiple AZs using S3. Use cases: disaster recovery, data migration, volume resizing, consistent production backups.

**Incremental:** only changed blocks saved after the initial snapshot. Each snapshot still appears as a full point-in-time copy — AWS manages the chain automatically.

| Snapshot | What's stored |
|----------|---------------|
| Initial | Full copy of all data blocks |
| Subsequent | Only blocks changed since last snapshot |
| Deletion | Only data unique to that snapshot removed; shared data preserved |

New volumes created from a snapshot are an exact copy of the original at snapshot time.

**Customer responsibility (shared responsibility model):** schedule and manage regular snapshots, monitor costs, delete unnecessary snapshots, encrypt sensitive data, verify integrity, test restoration procedures.

#### Amazon Data Lifecycle Manager

Automates creation, retention, and deletion of EBS snapshots. Schedules snapshots during off-peak hours, automatically deletes outdated backups, enforces retention rules for compliance.

Configure via EC2 console, API, AWS CLI, SDKs, or [[CloudFormation]].

**Policy setup steps:**
1. Create EBS snapshot policy
2. Select target resource type (EBS volume or EC2 instance)
3. Exclude volumes (root, data, or none)
4. Set custom schedule (frequency + retention count)
5. Apply additional actions: tags, archiving, fast snapshot restore, cross-region copy, cross-account sharing

## Where is the root volume?

Default: **EBS-backed.** The root volume is an EBS volume (attached at `/dev/xvda` or `/dev/sda1`). All current [[Compute#AMI — Amazon Machine Image|AMIs]] (Amazon Linux, Ubuntu, etc.) use EBS-backed root volumes.

Instance store-backed [[Compute#AMI — Amazon Machine Image|AMIs]] exist (root on ephemeral storage) but are legacy and rarely used today.

## Instance Store vs. EBS — Comparison

|                 | EC2 Instance Store                  | Amazon EBS                            |
| --------------- | ----------------------------------- | ------------------------------------- |
| **Persistence** | Lost on stop/terminate              | Survives stop/terminate               |
| **Type**        | Physically attached (local)         | Network-attached                      |
| **Best for**    | Temp buffers, caches, scratch       | Databases, OS volumes, long-term data |
| **Backup**      | None (ephemeral)                    | EBS Snapshots                         |
| **Root volume** | Legacy (instance store-backed [[Compute#AMI — Amazon Machine Image\|AMIs]]) | Default for all modern [[Compute#AMI — Amazon Machine Image\|AMIs]] |

---

## Amazon Simple Storage Service (S3)

Fully managed, highly-available **object storage** service. 99.999999999% (11 nines) durability. Supports versioning, lifecycle management, and multiple storage classes. Files stored as **objects** in **buckets**; objects range from bytes to terabytes.

Integrates with most AWS services. Use cases: backups, data lakes, static website hosting, media delivery, archiving, compliance data retention.

### S3 Objects

Fundamental unit of storage. Each object contains:

| Component | Description |
|-----------|-------------|
| **Data** | The file content itself |
| **Key** | Unique identifier (like a file name) within a bucket |
| **Metadata** | System and user-defined metadata |
| **Version ID** | Populated when versioning is enabled |
| **Access control** | Permissions on the object |

### S3 Buckets

Container for objects. Key properties:

- **Globally unique name** across all of AWS
- Created in a specific **Region**
- Virtually unlimited objects per bucket
- Access control and policies applied at bucket level
- Configurable: versioning, logging, access permissions

### Object Lifecycle Management

S3 lifecycle policies **automatically transition objects** between storage classes or delete them based on defined rules, eliminating manual tier management.

Two action types:

- **Transition actions** — move objects to a cheaper storage class after N days (e.g. → Standard-IA after 30 days → Glacier Deep Archive after 1 year)
- **Expiration actions** — permanently delete objects after N days

Example flow: object uploaded → Standard → Standard-IA at 30 days → Glacier Instant Retrieval at 90 days → deleted at 1 year.

Common use cases:
- **Periodic logs** — keep for a week or month, then delete automatically
- **Aging data** — frequently accessed at first, then archived as access drops off, then deleted per compliance requirements

### Use Cases

| Category | Examples |
|----------|----------|
| Content delivery | Static websites, media files |
| Application data | Backup storage, app assets |
| Analytics | Data lakes, log archiving |
| Compliance | Long-term data retention |

### Security

**Private by default** — all buckets and objects are private unless explicitly made public. Granular access control via bucket policies, IAM policies, and ACLs.

### S3 Storage Classes

S3 offers multiple storage classes optimized for different access patterns, cost requirements, and resiliency needs.

| Storage Class                     | Access Pattern                   | Retrieval                                  | Key Use Case                                                          |
| --------------------------------- | -------------------------------- | ------------------------------------------ | --------------------------------------------------------------------- |
| **S3 Standard**                   | Frequent                         | Immediate                                  | General-purpose: dynamic websites, CDN, big data                      |
| **S3 Intelligent-Tiering**        | Unknown / changing               | Immediate                                  | Auto-moves to cheapest tier based on access frequency                 |
| **S3 Standard-IA**                | Infrequent, rapid when needed    | Immediate                                  | Long-term backups, disaster recovery                                  |
| **S3 One Zone-IA**                | Infrequent, single AZ OK         | Immediate                                  | Secondary backups, easily recreatable data                            |
| **S3 Express One Zone**           | Very frequent, latency-sensitive | Single-digit ms                            | High-performance apps; 10× faster, 80% lower request cost vs Standard |
| **S3 Glacier Instant Retrieval**  | Rarely accessed                  | Milliseconds                               | Archives needing instant access; up to 68% cheaper than Standard-IA   |
| **S3 Glacier Flexible Retrieval** | 1–2× per year                    | 1–5 min (expedited) / 5–12 hr (bulk, free) | Backup, disaster recovery, offsite storage                            |
| **S3 Glacier Deep Archive**       | Once or twice per year           | 12 hr default                              | Regulatory compliance retention (7–10+ years)                         |
| **S3 Outposts**                   | On-premises                      | Immediate (local)                          | Local data residency; uses standard S3 APIs on AWS Outposts hardware  |

**S3 Intelligent-Tiering tiers:** frequent access → infrequent access → archive instant access. No retrieval fees; small monthly monitoring fee per object.

Default storage class when no class is specified: **S3 Standard**.

---

← [[Index]] · [[Compute]] · [[Home]]
