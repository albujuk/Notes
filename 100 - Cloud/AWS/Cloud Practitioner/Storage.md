---
domain: aws
track: cloud-practitioner
topic: storage
type: note
tags:
  - aws
  - cloud-practitioner
  - storage
---
# Storage

AWS storage spans three fundamental types:

| Type | Services | Key Trait |
|------|----------|-----------|
| **Block** | EC2 Instance Store, EBS | Low-latency attached volumes; one instance at a time |
| **Object** | S3 | Unlimited scale, 11 nines durability, HTTP access |
| **File** | EFS, FSx | Shared filesystem over NFS / SMB; multi-instance access |
| **Hybrid** | Storage Gateway | Bridge on-premises environments to AWS storage |

## Notes

- [[BlockStorage]]: EC2 Instance Store, EBS, EBS snapshots, Data Lifecycle Manager
- [[100 - Cloud/AWS/Cloud Practitioner/S3]]: Objects, buckets, storage classes, lifecycle management
- [[FileStorage]]: EFS, FSx (Windows File Server, NetApp ONTAP, OpenZFS, Lustre)
- [[StorageGateway]]: Hybrid cloud; File, Volume, and Tape gateway types

---

→ [aws.amazon.com: Storage Products](https://aws.amazon.com/products/storage/)

← [[100 - Cloud/AWS/Cloud Practitioner/README|Cloud Practitioner]] · [[Compute]]
