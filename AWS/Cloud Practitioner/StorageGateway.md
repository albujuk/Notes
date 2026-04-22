---
domain: aws
track: cloud-practitioner
topic: storage
type: note
tags:
  - aws
  - cloud-practitioner
  - storage
  - hybrid-cloud
  - storage-gateway
---
# Storage Gateway

Hybrid cloud storage service that integrates on-premises environments with AWS Cloud storage. Extends local storage to the cloud while maintaining low-latency access to frequently used data.

**Common use cases:**
- Move backups to the cloud
- On-premises file shares backed by cloud storage
- Low-latency access to AWS data for on-premises applications

## Gateway Types

| Type | Protocol | Backed By | Use Case |
|------|----------|-----------|----------|
| **File Gateway** | NFS / SMB | S3 | On-premises apps access S3 as a file share |
| **Volume Gateway** | iSCSI (block) | S3 + EBS snapshots | Block storage with cloud backup; two modes: Stored (primary on-prem) and Cached (primary in S3) |
| **Tape Gateway** | iSCSI VTL | S3 / S3 Glacier | Replace physical tape backup with virtual tapes |

### File Gateway
Presents S3 buckets as NFS or SMB file shares. Data transferred to S3 and cached locally for low-latency reads.

### Volume Gateway
Provides iSCSI block storage backed by cloud snapshots.
- **Stored mode** — primary data on-prem, async backup to S3 as EBS snapshots
- **Cached mode** — primary data in S3, frequently accessed data cached on-prem

### Tape Gateway
Virtual tape library (VTL) interface. Replaces physical tape infrastructure. Virtual tapes stored in S3; archived tapes moved to S3 Glacier or Glacier Deep Archive.

---

← [[Index]] · [[Storage]]