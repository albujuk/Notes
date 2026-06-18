---
domain: aws
track: solutions-architect-associate
topic: analytics
type: note
tags:
  - aws
  - solutions-architect-associate
  - analytics
  - athena
  - redshift
  - opensearch
  - emr
  - quicksight
---

# Analytics & Data Services

> For foundational analytics concepts, see [[100 - Cloud/AWS/Cloud Practitioner/Analytics|Cloud Practitioner: Analytics]].

---

## Amazon Athena

**Serverless** query service to analyze data stored in Amazon S3. Uses standard SQL language to query the files (built on Presto).

**Supported formats:** CSV, JSON, ORC, Avro, and Parquet

**Pricing:** $5.00 per TB of data scanned

**Use cases:**
- Business intelligence / analytics / reporting
- Analyze & query VPC Flow Logs, ELB Logs, CloudTrail trails
- Commonly used with Amazon QuickSight for reporting/dashboards

> [!tip] Exam Tip
> Analyze data in S3 using serverless SQL → use Athena

### Performance Improvement

- Use **columnar data** for cost-savings (less scan): Apache Parquet or ORC recommended
- Use Glue to convert your data to Parquet or ORC
- **Compress data** for smaller retrievals (bzip2, gzip, lz4, snappy, zlib, zstd)
- **Partition datasets** in S3 for easy querying on virtual columns:
  ```
  s3://yourBucket/pathToTable
  /<PARTITION_COLUMN_NAME>=<VALUE>
  /<PARTITION_COLUMN_NAME>=<VALUE>
  /<PARTITION_COLUMN_NAME>=<VALUE>
  ```
  Example: `s3://athena-examples/flight/parquet/year=1991/month=1/day=1/`
- Use **larger files** (> 128 MB) to minimize overhead

### Federated Query

Allows you to run SQL queries across data stored in relational, non-relational, object, and custom data sources (AWS or on-premises).

- Uses **Data Source Connectors** that run on AWS Lambda to run Federated Queries (e.g., CloudWatch Logs, DynamoDB, RDS)
- Store the results back in Amazon S3

---

## Amazon Redshift

Amazon Redshift is built on cloud economics that scale with your usage, powering modern analytics and autonomous agentic AI workloads on your data warehouse. Redshift delivers up to 2.2x better price-performance and 7x better throughput than other cloud data warehouses.

**Key characteristics:**
- Based on PostgreSQL, but **not used for OLTP**
- It's **OLAP** (online analytical processing, analytics and data warehousing)
- 10x better performance than other data warehouses, scale to PBs of data
- **Columnar storage** of data (instead of row-based) & parallel query engine
- Two modes: **Provisioned cluster** or **Serverless cluster**
- Has a SQL interface for performing the queries
- BI tools such as Amazon QuickSight or Tableau integrate with it
- **vs Athena:** faster queries / joins / aggregations thanks to indexes

### Redshift Architecture

- **Leader node:** query planning, results aggregation
- **Compute node:** performing the queries, send results to leader
- **Provisioned mode:** choose instance types in advance, can reserve instances for cost savings

### Redshift Spectrum

Efficiently query and retrieve structured and semi-structured data from files in Amazon S3 **without loading the data into Redshift tables**.

- Employs massive parallelism to run very fast against large datasets
- Much of the processing occurs in the Redshift Spectrum layer
- Most data remains in Amazon S3
- Multiple clusters can concurrently query the same dataset in S3 without copies

### Backup & Recovery

- Redshift has **Multi-AZ** mode for some clusters
- **Snapshots** are point-in-time backups of a cluster, stored internally in S3
- Snapshots are **incremental** (only what has changed is saved)
- You can restore a snapshot into a new cluster
- **Automated:** every 8 hours, every 5 GB, or on a schedule. Set retention between 1 to 35 days
- **Manual:** snapshot is retained until you delete it
- You can configure Amazon Redshift to automatically copy snapshots (automated or manual) of a cluster to another AWS Region

---

## Amazon OpenSearch

Amazon OpenSearch is the successor to Amazon ElasticSearch.

**Key characteristics:**
- In DynamoDB, queries only exist by primary key or indexes
- With OpenSearch, you can **search any field**, even partial matches
- Common to use OpenSearch as a **complement to another database**
- Two modes: **managed cluster** or **serverless cluster**
- Does not natively support SQL (can be enabled via a plugin)
- **Ingestion** from Kinesis Data Firehose, AWS IoT, and CloudWatch Logs
- **Security** through Cognito & IAM, KMS encryption, TLS
- Comes with **OpenSearch Dashboards** (visualization)

---

## Amazon EMR

**EMR** stands for "Elastic MapReduce". Helps creating Hadoop clusters (Big Data) to analyze and process vast amounts of data.

**Key characteristics:**
- Clusters can be made of hundreds of EC2 instances
- Comes bundled with **Apache Spark, HBase, Presto, Flink**
- Takes care of all the provisioning and configuration
- **Auto-scaling** and integrated with Spot instances
- **Use cases:** data processing, machine learning, web indexing, big data

### Node Types

- **Master Node:** manage the cluster, coordinate, manage health (long-running)
- **Core Node:** run tasks and store data (long-running)
- **Task Node** (optional): just to run tasks (usually Spot)

### Purchasing Options

- **On-demand:** reliable, predictable, won't be terminated
- **Reserved** (min 1 year): cost savings (EMR will automatically use if available)
- **Spot Instances:** cheaper, can be terminated, less reliable

Can have **long-running cluster** or **transient** (temporary) cluster.

---

## Amazon QuickSight

**Serverless** machine learning-powered business intelligence service to create interactive dashboards.

**Key characteristics:**
- Fast, automatically scalable, embeddable, with **per-session pricing**
- **Use cases:** business analytics, building visualizations, ad-hoc analysis, business insights
- **Integrated** with RDS, Aurora, Athena, Redshift, S3
- **In-memory computation** using SPICE engine if data is imported into QuickSight
- **Enterprise edition:** possibility to setup Column-Level security (CLS)

### Dashboard & Analysis

- Define **Users** (standard versions) and **Groups** (enterprise version)
- These users & groups only exist within QuickSight, not IAM
- A **dashboard:**
  - Is a **read-only snapshot** of an analysis that you can share
  - Preserves the configuration of the analysis (filtering, parameters, controls, sort)
  - You can share the analysis or the dashboard with Users or Groups
  - To share a dashboard, you must first **publish** it
  - Users who see the dashboard can also see the underlying data

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
