---
domain: aws
track: solutions-architect-associate
topic: database
type: note
tags:
  - aws
  - solutions-architect-associate
  - database
  - rds
  - rds-custom
  - multi-az
  - read-replicas
  - disaster-recovery
---

# RDS: Relational Database Service

Managed relational database service. AWS handles provisioning, patching, backups, and scaling.

> For foundational RDS concepts (key features, cost model, use cases), see [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon RDS (Relational Database Service)|Cloud Practitioner: RDS]].

## Supported Engines

PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL Server, IBM DB2, [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon Aurora|Amazon Aurora]] (AWS proprietary).

## Key Features

- Automated provisioning and OS patching
- Continuous backups with point-in-time restore
- Monitoring dashboards
- [[#Read Replicas|Read replicas]] for improved read performance
- [[#Multi-AZ|Multi-AZ]] setup for disaster recovery
- Maintenance windows for upgrades
- Vertical and horizontal scaling
- Storage backed by [[100 - Cloud/AWS/Solutions Architect Associate/EBS|EBS]]

**You cannot SSH into RDS instances.**

---

## Read Replicas vs Multi-AZ

### Read Replicas

Up to **15 replicas**. Replication is **asynchronous**.

| Characteristic | Detail |
|----------------|--------|
| Replication | Async |
| Promotion | Replicas can be promoted to a standalone DB |
| App changes | App must update connection string to use replicas |
| Use case | Read-only queries (`SELECT` statements); reduces read load on primary |
| Network costs | Cross-AZ replication is free; cross-Region replication incurs data transfer costs |

### Multi-AZ

Primary use is **disaster recovery**. Replication is **synchronous**.

| Characteristic | Detail |
|----------------|--------|
| Replication | Sync |
| DNS | Single DNS name; automatic failover to standby |
| Availability | Increased availability with no manual app intervention |
| Failover triggers | Loss of AZ, network failure, instance or storage failure |
| Scaling | Not used for scaling |

Read replicas can themselves be configured as Multi-AZ for disaster recovery.

---

## RDS Custom

Managed Oracle and Microsoft SQL Server with **OS and database customization**.

| | RDS | RDS Custom |
|--|-----|------------|
| **Management** | Entire database and OS managed by AWS | Full admin access to underlying OS and database |
| **Access** | No SSH/OS access | SSH or SSM Session Manager to underlying EC2 instance |

With RDS Custom you can:

- Configure OS and database settings
- Install patches
- Enable native features

Deactivate **Automation Mode** before performing customizations. Take a DB snapshot first.

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
