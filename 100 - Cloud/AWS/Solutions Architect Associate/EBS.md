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

## EBS Volume Types

Six volume types, characterized by **size, throughput, and IOPS**. Only SSD volumes can be used as boot volumes.

### SSD Volumes

|                       | **Amazon EBS General Purpose SSD**                                                                                                |                      | **Amazon EBS Provisioned IOPS SSD**                                                                       |                                                                            |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------- | -------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Volume type**       | gp3                                                                                                                               | gp2                  | io2 Block Express                                                                                         | io1                                                                        |
| **Durability**        | 99.8% - 99.9%                                                                                                                     | 99.8% - 99.9%        | 99.999%                                                                                                   | 99.8% - 99.9%                                                              |
| **Volume size**       | 1 GiB - 64 TiB                                                                                                                    | 1 GiB - 16 TiB       | 4 GiB - 64 TiB                                                                                            | 4 GiB - 16 TiB                                                             |
| **Max IOPS**          | 80,000 (25.6 KiB I/O)                                                                                                             | 16,000 (16 KiB I/O)  | 256,000 (16 KiB I/O)                                                                                      | 64,000 (16 KiB I/O)                                                        |
| **Max throughput**    | 2,000 MiB/s                                                                                                                       | 250 MiB/s            | 4,000 MiB/s                                                                                               | 1,000 MiB/s                                                                |
| **Multi-Attach**      | Not supported                                                                                                                     | Not supported        | Supported                                                                                                 | Supported                                                                  |
| **NVMe reservations** | Not supported                                                                                                                     | Not supported        | Supported                                                                                                 | Not supported                                                              |
| **Boot volume**       | Supported                                                                                                                         | Supported            | Supported                                                                                                 | Supported                                                                  |
| **Use cases**         | Transactional workloads, virtual desktops, medium single-instance databases, low-latency interactive apps, boot volumes, dev/test | Same as gp3 (legacy) | Consistent sub-millisecond latency (<500 μs avg), sustained IOPS, >80,000 IOPS or >2,000 MiB/s throughput | Sustained IOPS performance, >16,000 IOPS, I/O-intensive database workloads |

**gp3 vs gp2 key difference:** gp3 lets you set IOPS and throughput **independently** (baseline: 3,000 IOPS, 125 MiB/s; scalable to 16,000 IOPS and 1,000 MiB/s). gp2 **links** IOPS to volume size (3 IOPS per GB, maxing at 16,000 IOPS at ~5,334 GB).

### HDD Volumes

|                                     | **Throughput Optimized HDD volumes**      | **Cold HDD volumes**                            |
| ----------------------------------- | ----------------------------------------- | ----------------------------------------------- |
| **Volume type**                     | st1                                       | sc1                                             |
| **Durability**                      | 99.8% - 99.9%                             | 99.8% - 99.9%                                   |
| **Use cases**                       | Big data, data warehouses, log processing | Infrequently accessed data, lowest cost storage |
| **Volume size**                     | 125 GiB - 16 TiB                          | 125 GiB - 16 TiB                                |
| **Max IOPS per volume** (1 MiB I/O) | 500                                       | 250                                             |
| **Max throughput per volume**       | 500 MiB/s                                 | 250 MiB/s                                       |
| **Multi-Attach**                    | Not supported                             | Not supported                                   |
| **Boot volume**                     | Not supported                             | Not supported                                   |

> For foundational EBS concepts (AZ constraint, snapshots, delete-on-termination), see [[100 - Cloud/AWS/Cloud Practitioner/BlockStorage#Amazon Elastic Block Store (EBS)|Cloud Practitioner: EBS]].

## EBS Multi-Attach

Attach the same EBS volume to **multiple EC2 instances in the same AZ**. Each instance has full read and write permissions to the high-performance volume.

- Up to **16 EC2 instances** at a time
- Must use a **cluster-aware file system** (not XFS, EXT4, etc.) such as **OCFS2** (Oracle Cluster File System) or **GFS2** (Global File System 2)
- Applications must manage concurrent write operations
- **Use cases:**
  - Higher application availability in clustered Linux applications (e.g. Teradata)
  - Shared storage for SAP HANA scale-out deployments

> Supported on **io2 Block Express** and **io1** volume types only. See [[#SSD Volumes|SSD Volumes table]].

## EBS vs. EC2 Instance Store

|                 | EBS                                    | EC2 Instance Store                              |
| --------------- | -------------------------------------- | ----------------------------------------------- |
| **Connection**  | Network-attached                       | Physically attached to the EC2 host             |
| **Persistence** | Survives stop/terminate                | Data lost on stop                               |
| **Performance** | Network latency                        | Better I/O performance (direct physical access) |
| **Best for**    | Databases, OS volumes, persistent data | Cache, temp data, buffers                       |

> For foundational coverage (use cases, Data Lifecycle Manager), see [[100 - Cloud/AWS/Cloud Practitioner/BlockStorage#EC2 Instance Store|Cloud Practitioner: Instance Store]].

## EBS Encryption

Encrypts everything: data at rest, data in transit (between volume and instance), and snapshots. Nearly no impact on latency or performance. Considered a best practice to always enable.

To encrypt an existing unencrypted volume: create a snapshot, then create a new encrypted volume from that snapshot.

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
