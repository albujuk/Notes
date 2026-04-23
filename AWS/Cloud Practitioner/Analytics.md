---
domain: aws
track: cloud-practitioner
topic: analytics
type: note
tags:
  - aws
  - cloud-practitioner
  - analytics
  - data-pipeline
  - etl
  - kinesis
  - firehose
  - redshift
  - glue
  - emr
  - athena
  - quicksight
  - opensearch
---

# AWS Analytics and Data Pipeline Services

Data moves through a pipeline: ingestion → storage → cataloging → processing → analysis. AWS provides managed services for each stage.

---

## Data Pipelines and ETL

Both AI/ML and traditional analytics need clean, accessible data. ETL processes handle this:

1. **Extract** — pull data from source systems
2. **Transform** — convert to a consistent, usable format
3. **Load** — write to a destination (data warehouse, analytics platform)

Data pipelines automate and repeat ETL at scale.

---

## Data Ingestion

Move data from sources into storage. Use **real-time** when data needed immediately; **batch** when latency is tolerable.

### Amazon Kinesis Data Streams

Real-time ingestion of terabytes from applications, streams, and sensors. Serverless with automatic provisioning and scaling (on-demand mode).

### Amazon Data Firehose

Near real-time ingestion. Fully managed, auto-scaling. Delivers data within seconds to data lakes, warehouses, and analytics services.

### Kinesis vs Firehose

|                 | Kinesis Data Streams              | Data Firehose                         |
| --------------- | --------------------------------- | ------------------------------------- |
| **Latency**     | Real-time (milliseconds)          | Near real-time (seconds)              |
| **Management**  | You manage consumers/shards       | Fully managed, no config              |
| **Processing**  | You write consumer code           | Built-in delivery, no code            |
| **Destination** | Any — your app reads the stream   | Fixed: S3, Redshift, OpenSearch, etc. |
| **Retention**   | Data stays in stream (1–365 days) | No retention — deliver and done       |
| **Use when**    | Process/react to data in flight   | Just need to land data somewhere fast |

---

## Data Storage

### Amazon S3

Popular choice for data lakes. Object storage for virtually any amount of structured or unstructured data. Fully elastic. → [[S3]]

### Amazon Redshift

Fully managed data warehouse. Stores petabytes of structured or semi-structured data. Pay-as-you-go pricing for cost-effective large-dataset analysis. Columnar storage and massively parallel processing make it ideal for frequent, high-performance analytical SQL workloads.

---

## Data Cataloging

### AWS Glue Data Catalog

Centralized, scalable metadata repository. Enhances data discovery and improves pipeline efficiency by delivering metadata to data stores and analytics services.

---

## Data Processing

### AWS Glue

Fully managed ETL service. Uses the [[#AWS Glue Data Catalog]] to access source metadata and inform ETL script transformations.

### Amazon EMR

Large-scale data processing. Handles infrastructure provisioning, cluster management, and scaling. Supports Apache Spark, Hadoop, and Hive. Ideal for organizations with existing big data expertise. Also serves as [[AI#AWS ML Infrastructure|ML infrastructure]] for custom model training workloads.

---

## Data Analysis and Visualization

### Amazon Athena

Serverless SQL queries on relational, non-relational, object, and custom data sources. Accesses data in [[S3]], on-premises, or multi-cloud. Pay only for queries you run.

**[[#Amazon Redshift]]** is also used here for high-performance complex analytical SQL at scale.

### Amazon QuickSight

Interactive dashboards and reports for technical and non-technical users, no infrastructure to manage. [[AI#Amazon Q|Amazon Q]] in QuickSight enables natural language queries for business analysts.

### Amazon OpenSearch Service

Search via keyword matching or natural language queries. Unified dashboards for real-time visualization, log analysis, and monitoring of traces and metrics.

---

## Data Analytics

Data analytics transforms raw historical data to uncover insights and trends.

**Use cases:**
- Loan companies explaining lending decisions to customers
- Medical researchers analyzing clinical trial data through hypothesis testing
- Insurance companies making risk assessment models transparent for regulators

---

← [[Index]] · [[Home]]
