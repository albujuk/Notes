---
domain: aws
track: solutions-architect-associate
topic: storage
type: note
tags:
  - aws
  - solutions-architect-associate
  - storage
  - ebs
  - ebs-snapshots
  - ec2
---

# EBS: Elastic Block Store

EBS volumes can only exist in **one Availability Zone**, so a volume in az-1 cannot connect to an instance in az-2.

Think of EBS volumes as **network USB sticks**: they can be attached and detached. They have provisioned capacity that can be increased over time.

**Delete on termination** attribute: on by default for EBS root volumes, off for external (additional) volumes.

> For foundational EBS concepts (use cases, benefits, Data Lifecycle Manager), see [[100 - Cloud/AWS/Cloud Practitioner/BlockStorage#Amazon Elastic Block Store (EBS)|Cloud Practitioner: EBS]].

## Snapshots

Point-in-time backup of an EBS volume. No need to detach EBS from EC2 before snapshotting, but it is recommended for consistency. Snapshots can be copied across regions and AZs.

### EBS Snapshot Archive

- Moves the snapshot to an archive tier, reducing cost by **75%**
- Takes **24 to 72 hours** to restore

### EBS Recycle Bin

Setup rules to retain deleted snapshots to recover from accidental deletion. Retention period ranges from **1 day to 1 year**.

### Fast Snapshot Restore

Forces full initialization of the snapshot so there is no latency on first use. Costs more.

## EBS vs. EC2 Instance Store

| | EBS | EC2 Instance Store |
|---|-----|--------------------|
| **Connection** | Network-attached | Physically attached to the EC2 host |
| **Persistence** | Survives stop/terminate | Data lost on stop |
| **Performance** | Network latency | Better I/O performance (direct physical access) |
| **Best for** | Databases, OS volumes, persistent data | Cache, temp data, buffers |

> For foundational coverage (use cases, Data Lifecycle Manager), see [[100 - Cloud/AWS/Cloud Practitioner/BlockStorage#EC2 Instance Store|Cloud Practitioner: Instance Store]].

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
