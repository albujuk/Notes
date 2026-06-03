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

PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL Server, IBM DB2, [[#Amazon Aurora|Amazon Aurora]] (AWS proprietary).

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

## Amazon Aurora

Proprietary AWS managed relational database (not open source). MySQL- and PostgreSQL-compatible; existing drivers work as if Aurora were a standard MySQL or PostgreSQL database.

> For foundational Aurora concepts (performance claims, use cases), see [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon Aurora|Cloud Practitioner: Aurora]].

### Performance

- Up to **5x** the throughput of standard MySQL on RDS, **3x** PostgreSQL on RDS
- Costs ~20% more than standard RDS but is more efficient
- Designed to reduce unnecessary I/O operations

### Storage Architecture

- **Shared storage volume** that auto-expands in 10 GB increments, up to **256 TB**
- **6 copies across 3 [[100 - Cloud/AWS/Cloud Practitioner/Global Infrastructure#Availability Zones|AZs]]**:
  - 4 copies needed for writes
  - 3 copies needed for reads
- Self-healing with peer-to-peer replication
- Storage striped across hundreds of volumes
- Continuous backup to [[100 - Cloud/AWS/Cloud Practitioner/S3|S3]]; point-in-time recovery

### High Availability and Replication

- One **master instance** takes writes
- Up to **15 Aurora Read Replicas** with **sub-10 ms replica lag** (faster than MySQL replication)
- Automated failover in less than **30 seconds**
- Support for **Cross Region Replication**

### Endpoints

- **Writer endpoint**: DNS name always pointing to the master; automatically redirects on failover
- **Reader endpoint**: connection-level load balancing across all read replicas; automatically tracks replicas as they scale; load balancing happens at the **connection level**, not the statement level

### Features

- Automatic failover
- Backup and recovery
- **Backtrack**: restore data at any point in time without using backups
- Push-button scaling
- Automated patching with zero downtime
- Advanced monitoring
- Routine maintenance
- Isolation, security, and industry compliance

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
