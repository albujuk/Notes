---
domain: aws
track: cloud-practitioner
topic: database
type: note
tags:
  - aws
  - cloud-practitioner
  - database
  - rds
  - aurora
  - dynamodb
  - elasticache
  - documentdb
  - neptune
  - relational
  - nosql
  - document
  - graph
  - caching
  - managed-service
---

# Databases

Managed database services. AWS handles routine tasks — backups, patching, hardware provisioning — so you focus on your application.

---

## Amazon RDS (Relational Database Service)

Managed relational database service. Supports multiple instance class types optimized for memory, performance, or I/O.

### Supported engines

[[#Amazon Aurora|Amazon Aurora]], MySQL, PostgreSQL, Microsoft SQL Server, MariaDB, Oracle Database.

### Key features

- **Multi-AZ deployment** — replicates data to a standby instance in a different [[Global Infrastructure#Availability Zones|Availability Zone]]; automatic failover on failure or maintenance with minimal downtime
- **Automated backups** — point-in-time recovery built in
- **DB snapshots** — manual full backups for specific point-in-time recovery or long-term archiving
- **Read replicas** — offload read traffic from the primary instance
- **Performance Insights** — real-time monitoring; identifies and helps resolve performance bottlenecks
- **Security** — VPC network isolation, encryption at rest, encryption in transit

### Scaling

Scale vertically (instance size) or horizontally (read replicas) as needed.

### Use cases

Web applications, enterprise workloads, e-commerce product inventories.

### Cost model

Pay-as-you-go for compute and storage consumed. No upfront hardware costs. Managed service reduces operational overhead (no manual patching, backup management).

---

## Amazon Aurora

Managed relational database built for the cloud. MySQL- and PostgreSQL-compatible. Designed to reduce unnecessary I/O operations while delivering higher throughput than standard engines.

- Up to **5×** the throughput of standard MySQL, **3×** PostgreSQL
- **Distributed storage** across multiple nodes — high performance and availability
- **Auto-scaling storage** — grows from 10 GB to 128 TB based on actual usage; no capacity planning needed
- **Continuous backup to [[S3]]** — point-in-time recovery
- **6 copies across 3 AZs** — 99.99% availability; automatic failure detection and traffic rerouting to healthy replicas with no data loss
- Encryption at rest, automated backups, continuous monitoring

### Use cases

Gaming applications, media and content management, real-time analytics.

---

## Amazon DynamoDB

Fully managed **NoSQL** database. Supports document and key-value data structures. Flexible schema — ideal for applications needing high performance and seamless scaling.

- **Single-digit millisecond** response times at any scale
- **Auto-scaling throughput** — scales up/down based on actual usage; consistent performance without manual intervention; no practical table size limit
- **99.999% availability** — data replicated across 3 facilities within each Region; multiple copies across Regions for fault tolerance
- **Encryption at rest and in transit** — automatic, with choice of encryption key type
- Pay only for resources used

### Use cases

Gaming platforms, financial services, mobile applications with global user bases.

---

## Amazon DocumentDB

Fully managed **document database** with MongoDB compatibility. Stores, queries, and indexes JSON-like documents with dynamic schemas — no predefined schema required.

- MongoDB-compatible: same tools, drivers, and queries work without changes
- Handles semistructured data that doesn't fit rigid relational tables
- Automatic scaling, continuous backup, encryption, enterprise-grade security
- Iterate schema freely — no migrations needed for schema changes

### Use cases

Content management systems, catalog and inventory management, user profile and personalization.

---

## Amazon Neptune

Fully managed **graph database**. Purpose-built for highly connected datasets where relationships between entities are as important as the data itself.

- Excels at traversing complex relationships (user connections, friend networks, interaction patterns) — difficult to model in relational DBs
- High availability with automatic failover and backups
- Maintains high performance as data complexity grows

### Use cases

Social network connection mapping, fraud detection, search and recommendation engines.

---

## Amazon ElastiCache

Fully managed **in-memory caching** service. Compatible with Redis, Valkey, and Memcached — same tools and configs, no infrastructure to manage. Automatically detects and replaces failed nodes.

- **Sub-millisecond latency** — data lives in memory, not on disk
- **Auto-scaling** — add/remove nodes as demand changes
- **Multi-AZ replication** — primary + replica nodes across AZs; automatic failover promotes replica to primary within minutes
- **Encryption** — at rest (disk + backups) and in transit (TLS)

### Use cases

Session data management, database query caching, gaming leaderboards.

---

## AWS Backup

Centralized backup management across AWS resources and on-premises deployments. Single dashboard to monitor and manage all backups — eliminates juggling multiple backup strategies per service.

- Supports: [[BlockStorage#EBS|EBS]] volumes, [[FileStorage#Amazon EFS|EFS]] file systems, RDS, DynamoDB, and more
- Flexible scheduling, encryption, cross-Region backup for disaster recovery
- Automates data protection policies for compliance consistency

### Use cases

Centralized disaster recovery, compliance backup policies, consolidating multiple backup processes.

---

## Amazon Redshift

Fully managed **data warehouse** — optimized for analytical queries on large historical datasets, not OLTP workloads. Columnar storage and massively parallel processing. See [[Analytics#Amazon Redshift|Analytics]] for full coverage including its role in data pipelines and visualization.

---

← [[100 - Cloud/AWS/Cloud Practitioner/README|Cloud Practitioner]]