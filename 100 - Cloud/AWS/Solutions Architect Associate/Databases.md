---
domain: aws
track: solutions-architect-associate
topic: database
type: note
tags:
  - aws
  - solutions-architect-associate
  - database
  - documentdb
  - nosql
  - mongodb
---

# Databases (Non-Relational)

> For relational databases (RDS, Aurora), see [[100 - Cloud/AWS/Solutions Architect Associate/RDS|RDS]].
> For foundational database concepts, see [[100 - Cloud/AWS/Cloud Practitioner/Database|Cloud Practitioner: Database]].

---

## Amazon DocumentDB

MongoDB-compatible managed database service for running MongoDB workloads on AWS.

> [!note] Update from Cloud Practitioner level
> DocumentDB was previously not serverless and not global. It now offers **serverless mode** (available since version 5.0, extended to 8.0).

### Key Features

- **MongoDB compatibility**: works with existing MongoDB drivers and tools
- **Serverless mode**: auto-scaling based on actual usage (since v5.0)
- **Global support**: can now be deployed globally (recently extended to v8.0)
- **Managed service**: AWS handles provisioning, patching, backups, and scaling
- **Storage**: auto-scales in 10 GB increments up to 64 TB
- **Replication**: 6 copies across 3 AZs

### Use Cases

- Content management and catalogs
- User profiles and preferences
- Real-time analytics
- Mobile and gaming backends

### Migration

Use [[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#Migration Services|AWS DMS and SCT]] to migrate from MongoDB to DocumentDB.

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]