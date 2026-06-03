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
  - aurora-serverless
  - aurora-global-database
  - aurora-ml
  - babelfish
  - backup-restore
  - encryption
  - iam-auth
  - aurora-fast-cloning
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

## Backup & Restore

### RDS Automated Backups

- Daily full backup during the backup window
- Transaction logs backed up every **5 minutes**
- Point-in-time restore from oldest backup to **5 minutes ago**
- Retention: **1 to 35 days** (set to 0 to disable)

### RDS Manual Snapshots

- Manually triggered by the user
- Retained for as long as you want

> [!tip] Cost trick
> A stopped RDS database still incurs storage costs. If stopping for a long time, snapshot and delete the instance instead, then restore when needed.

### Aurora Automated Backups

- Retention: **1 to 35 days**, **cannot be disabled**
- Point-in-time recovery

### Aurora Manual Snapshots

- Manually triggered by the user
- Retained for as long as you want

### Restoring from S3

| Target | Process |
|--------|---------|
| **RDS MySQL** | Backup on-prem DB → store on [[100 - Cloud/AWS/Cloud Practitioner/S3\|S3]] → restore onto new RDS MySQL instance |
| **Aurora MySQL** | Backup on-prem DB using **Percona XtraBackup** → store on S3 → restore onto new Aurora MySQL cluster |

Restoring any RDS or Aurora backup/snapshot **creates a new database**.

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

### Aurora Auto Scaling

When CPU usage increases on read endpoints, Aurora Auto Scaling adds new read replica instances to the scaling group. Requests are distributed across all replicas via the [[#Endpoints|reader endpoint]].

### Custom Endpoints

Custom endpoints route specific types of requests to specific instances. Creating multiple custom endpoints is good practice when you need fine-grained traffic routing (e.g., analytics queries to a dedicated subset of replicas). Custom endpoints affect the behavior of the reader endpoint.

### Aurora Serverless

- Automated database instantiation and auto-scaling based on actual usage
- Good for infrequent, intermittent, or unpredictable workloads
- No capacity planning needed
- Pay per second; can be more cost-effective than provisioned Aurora

### Aurora Global Database

- **1 primary Region** (read/write)
- **Up to 10 secondary (read-only) Regions**, replication lag < 1 second
- Up to **16 Read Replicas per secondary Region**
- Decreases global read latency
- Promoting a secondary Region for disaster recovery has an **RTO < 1 minute**
- Typical cross-region replication takes less than 1 second

Simpler alternative: **Aurora Cross Region Read Replicas** (useful for DR, easier to set up but less feature-rich).

### Aurora ML

- Add ML-based predictions to applications via SQL queries
- Optimized integration between Aurora and AWS ML services
- Supported services:
  - **Amazon SageMaker** (any ML model)
  - **Amazon Comprehend** (sentiment analysis)
- No ML experience required
- Use cases: fraud detection, ads targeting, sentiment analysis, product recommendations

### Babelfish for Aurora PostgreSQL

- Allows Aurora PostgreSQL to understand commands targeted for MS SQL Server (e.g., T-SQL)
- Microsoft SQL Server-based applications can work on Aurora PostgreSQL with little to no code changes
- Uses the same MS SQL Server client driver
- Migrate with [[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#Migration Services|AWS SCT and DMS]]

### Aurora Fast Cloning

- Create a new Aurora DB cluster from an existing one
- **Faster than snapshot & restore**
- Uses **copy-on-write protocol**: the new cluster initially shares the same data volume as the original (no copying needed)
- When updates are made to the new cluster, additional storage is allocated and only changed data is copied
- Very fast and cost-effective
- Useful for creating a staging database from production without impacting the production database

---

## Encryption & Security

### Encryption at Rest

- Database master and replicas encrypted using **[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS KMS (Key Management Service)|AWS KMS]]**
- Must be defined at launch time
- If the master is not encrypted, read replicas cannot be encrypted
- To encrypt an unencrypted database: snapshot → restore as encrypted

### Encryption in Transit

- TLS-ready by default
- Use AWS TLS root certificates client-side

### IAM Authentication

- Use IAM roles to connect to the database instead of username/password

### Network Security

- **Security Groups** control network access to RDS/Aurora
- No SSH available (except on [[#RDS Custom|RDS Custom]])

### Audit Logs

- Can be enabled and sent to [[100 - Cloud/AWS/Cloud Practitioner/Monitoring#Amazon CloudWatch|CloudWatch Logs]] for longer retention

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
