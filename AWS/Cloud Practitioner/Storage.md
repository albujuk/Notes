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

| Use case | Notes |
|----------|-------|
| Database hosting | Low-latency, consistent IOPS |
| App backup storage | Snapshots for point-in-time recovery |
| Dev environment cloning | Launch a snapshot as a new volume |

### Benefits

| Capability                | How it helps                                              |
| ------------------------- | --------------------------------------------------------- |
| **Data migration**        | Snapshot → restore in another AZ or region                |
| **Instance type changes** | Detach volume, attach to new instance type — no data loss |
| **Disaster recovery**     | Snapshots restorable in different regions                 |
| **Cost optimization**     | Modify volume type or size without downtime               |
| **Performance tuning**    | Switch volume type (gp3, io2, etc.) on the fly            |

## Where is the root volume?

Default: **EBS-backed.** The root volume is an EBS volume (attached at `/dev/xvda` or `/dev/sda1`). All current AMIs (Amazon Linux, Ubuntu, etc.) use EBS-backed root volumes.

Instance store-backed AMIs exist (root on ephemeral storage) but are legacy and rarely used today.

## Instance Store vs. EBS — Comparison

| | EC2 Instance Store | Amazon EBS |
|---|---|---|
| **Persistence** | Lost on stop/terminate | Survives stop/terminate |
| **Type** | Physically attached (local) | Network-attached |
| **Best for** | Temp buffers, caches, scratch | Databases, OS volumes, long-term data |
| **Backup** | None (ephemeral) | EBS Snapshots |
| **Root volume** | Legacy (instance store-backed AMIs) | Default for all modern AMIs |

---

← [[Index]] · [[Compute]] · [[Home]]
