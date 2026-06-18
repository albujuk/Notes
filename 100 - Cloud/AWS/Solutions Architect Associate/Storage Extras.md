---
domain: aws
track: solutions-architect-associate
topic: storage
type: note
tags:
  - aws
  - solutions-architect-associate
  - storage
  - snowball
  - fsx
  - storage-gateway
  - transfer-family
  - datasync
  - hybrid-cloud
---

# Storage Extras

> For foundational storage concepts, see [[100 - Cloud/AWS/Cloud Practitioner/Storage|Cloud Practitioner: Storage]] and [[100 - Cloud/AWS/Cloud Practitioner/StorageGateway|Cloud Practitioner: Storage Gateway]].

---

## AWS Snow Family

Highly-secure, portable devices to collect and process data at the edge, and migrate data into and out of AWS. Helps migrate up to **petabytes** of data.

### When to Use Snowball

- Limited connectivity
- Limited bandwidth
- High network cost
- Shared bandwidth (cannot maximize the line)
- Connection instability

> [!tip] Rule of thumb
> AWS Snowball is offline devices for data migration. If it takes more than a week to transfer over the network, use Snowball devices.

### Edge Computing

Process data while it is being created on an edge location (a truck on the road, a ship on the sea, etc.). These locations may have limited internet and no access to computing power.

Set up a **Snowball Edge** device for edge computing:

- **Snowball Edge Compute Optimized:** dedicated for compute use cases
- **Snowball Edge Storage Optimized:** dedicated for storage use cases
- Run **EC2 instances** or **Lambda functions** at the edge
- Use cases: preprocess data, machine learning, transcoding media

> [!warning] Snowball and Glacier
> Snowball **cannot** import to Glacier directly. You must use Amazon S3 first, in combination with an **S3 lifecycle policy** to transition to Glacier.

---

## Amazon FSx

Launch 3rd-party high-performance file systems on AWS. Fully managed service.

| FSx Type | Protocol | Use Case |
|----------|----------|----------|
| **FSx for Lustre** | Native Lustre | HPC, ML, video processing, financial modeling, EDA |
| **FSx for Windows** | SMB, NTFS | Windows file shares, AD integration |
| **FSx for NetApp ONTAP** | NFS, SMB, iSCSI | Lift-and-shift from ONTAP/NAS |
| **FSx for OpenZFS** | NFS (v3, v4, v4.1, v4.2) | Lift-and-shift from ZFS |

### FSx for Windows

- Fully managed Windows file system share drive
- Supports **SMB protocol** & **Windows NTFS**
- **Microsoft Active Directory** integration, ACLs, user quotas
- Can be mounted on Linux EC2 instances
- Supports Microsoft's **Distributed File System (DFS) Namespaces** (group files across multiple FS)
- Scale up to 10s of GB/s, millions of IOPS, 100s PB of data
- **Storage options:**
  - **SSD:** latency-sensitive workloads (databases, media processing, data analytics)
  - **HDD:** broad-spectrum workloads (home directory, CMS)
- Can be accessed from on-premises (VPN or Direct Connect)
- Can be configured as **Multi-AZ** (high availability)
- Data is backed up daily to S3

### FSx for Lustre

Lustre is a parallel distributed file system for large-scale computing. The name is derived from Linux + cluster.

- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies
- **Storage options:**
  - **SSD:** low-latency, IOPS-intensive workloads, small and random file operations
  - **HDD:** throughput-intensive workloads, large and sequential file operations
- **Seamless integration with S3:**
  - Can read S3 as a file system (through FSx)
  - Can write the output of computations back to S3 (through FSx)
- Can be used from on-premises servers (VPN or Direct Connect)

**Deployment options:**

| | Scratch File System | Persistent File System |
|--|---------------------|------------------------|
| **Purpose** | Temporary storage | Long-term storage |
| **Replication** | Not replicated | Replicated within same AZ |
| **Durability** | Data lost if file server fails | Replace failed files within minutes |
| **Burst** | 6x faster (200 MBps per TiB) | Standard burst |
| **Use case** | Short-term processing, optimize costs | Long-term processing, sensitive data |

### FSx for NetApp ONTAP

- Managed NetApp ONTAP on AWS
- File system compatible with **NFS, SMB, iSCSI** protocol
- Move workloads running on **ONTAP or NAS** to AWS
- Works with: Linux, Windows, MacOS, VMware Cloud on AWS, Amazon Workspaces & AppStream 2.0, EC2, ECS, EKS
- Storage shrinks or grows automatically
- Snapshots, replication, low-cost, compression, and data de-duplication
- Point-in-time instantaneous cloning (helpful for testing new workloads)

### FSx for OpenZFS

- Managed OpenZFS file system on AWS
- File system compatible with **NFS** (v3, v4, v4.1, v4.2)
- Move workloads running on ZFS to AWS
- Works with: Linux, Windows, MacOS, VMware Cloud on AWS, Amazon Workspaces & AppStream 2.0, EC2, ECS, EKS
- Up to **1,000,000 IOPS** with **< 0.5 ms latency**
- Snapshots, compression, and low-cost
- Point-in-time instantaneous cloning (helpful for testing new workloads)

---

## Hybrid Cloud for Storage

AWS pushes for hybrid cloud: part of your infrastructure on the cloud, part on-premises. Reasons include long cloud migrations, security requirements, compliance requirements, and IT strategy.

S3 is proprietary storage technology (unlike EFS/NFS), so exposing S3 data on-premises requires **AWS Storage Gateway**.

---

## AWS Storage Gateway

Bridge between on-premises data and cloud data.

**Use cases:** disaster recovery, backup & restore, tiered storage, on-premises cache & low-latency file access.

### S3 File Gateway

- Configured S3 buckets are accessible using **NFS** and **SMB** protocol
- **Most recently used data is cached** in the file gateway
- Supports S3 Standard, S3 Standard-IA, S3 One Zone-IA, S3 Intelligent-Tiering
- **Transition to S3 Glacier** using a lifecycle policy
- Bucket access using IAM roles for each File Gateway
- SMB protocol has integration with **Active Directory (AD)** for user authentication

### Volume Gateway

- Block storage using **iSCSI** protocol backed by S3
- Backed by EBS snapshots, which can help restore on-premises volumes
- **Cached volumes:** low-latency access to most recent data
- **Stored volumes:** entire dataset is on-premises, scheduled backups to S3

### Tape Gateway

- Some companies have backup processes using physical tapes
- With Tape Gateway, companies use the same processes but in the cloud
- **Virtual Tape Library (VTL)** backed by Amazon S3 and Glacier
- Back up data using existing tape-based processes (and iSCSI interface)
- Works with leading backup software vendors

---

## AWS Transfer Family

Fully-managed service for file transfers into and out of Amazon S3 or Amazon EFS.

**Supported protocols:**
- AWS Transfer for **FTP** (File Transfer Protocol)
- AWS Transfer for **FTPS** (File Transfer Protocol over SSL)
- AWS Transfer for **SFTP** (Secure File Transfer Protocol)

- Managed infrastructure, scalable, reliable, highly available (multi-AZ)
- Pay per provisioned endpoint per hour + data transfers in GB
- Store and manage users' credentials within the service
- Integrate with existing authentication systems (Microsoft Active Directory, LDAP, Okta, Amazon Cognito, custom)
- Usage: sharing files, public datasets, CRM, ERP

---

## AWS DataSync

Move large amounts of data to and from:

- **On-premises / other cloud to AWS** (NFS, SMB, HDFS, S3 API): **needs agent**
- **AWS to AWS** (different storage services): **no agent needed**

**Can synchronize to:**
- Amazon S3 (any storage class, including Glacier)
- Amazon EFS
- Amazon FSx (Windows, Lustre, NetApp, OpenZFS)

- Replication tasks can be scheduled hourly, daily, weekly
- File permissions and metadata are preserved (NFS POSIX, SMB)
- One agent task can use **10 Gbps**; can set up a **bandwidth limit**

---

## Storage Comparison

| Type | Services |
|------|----------|
| **Block** | Amazon EBS, EC2 Instance Store |
| **File** | Amazon EFS, Amazon FSx |
| **Object** | Amazon S3, Amazon Glacier |
| **Hybrid** | Storage Gateway (S3 File Gateway, FSx File Gateway, Volume Gateway, Tape Gateway) |
| **Transfer** | Transfer Family (FTP, FTPS, SFTP on top of S3 or EFS) |
| **Sync** | DataSync (scheduled sync from on-premises to AWS, or AWS to AWS) |
| **Migration** | Snowcone / Snowball / Snowmobile (physical, for large data transfers) |
| **Database** | For specific workloads, usually with indexing and querying |

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]