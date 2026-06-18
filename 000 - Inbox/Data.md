# Glue 
• Managed extract, transform, and load (ETL) service
• Useful to prepare and transform data for analytics
• Fully serverless service
used also with metadata
• Glue Job Bookmarks: prevent re-processing old data
• Glue DataBrew: clean and normalize data using pre-built transformation
• Glue Studio: new GUI to create, run and monitor ETL jobs in Glue
• Glue Streaming ETL (built on Apache Spark Structured Streaming):
	compatible with Kinesis Data Streaming, Kafka, MSK (managed Kafka)

# Lake Formation

• Data lake = central place to have all your data for analytics purposes
• Fully managed service that makes it easy to setup a data lake in days
• Discover, cleanse, transform, and ingest data into your Data Lake
• It automates many complex manual steps (collecting, cleansing, moving,
cataloging data, …) and de-duplicate (using ML Transforms)
• Combine structured and unstructured data in the data lake
• Out-of-the-box source blueprints: S3, RDS, Relational & NoSQL DB…
• Fine-grained Access Control for your applications (row and column-level)
• Built on top of AWS Glue


it has centralized perm for your data so you manage access in one place

it stores the data into one central s3


# Amazon Managed Service for Apache Flink
• Previously named: Kinesis Data Analytics for Apache Flink
• Flink (Java, Scala or SQL) is a framework for processing data streams
• Run any Apache Flink application on a managed cluster on AWS
• Provisioned compute resources, parallel computation, automatic scaling
• Application backups (implemented as checkpoints and snapshots)
• Use any Apache Flink programming features to transform data
• Important: Flink does not read from Amazon Data Firehose

# MSK

• Alternative to Amazon Kinesis
• Fully managed Apache Kafka on AWS
• Allow you to create, update, delete clusters
• MSK creates & manages Kafka brokers nodes & Zookeeper nodes for you
• Deploy the MSK cluster in your VPC, multi-AZ (up to 3 for HA)
• Automatic recovery from common Apache Kafka failures
• Data is stored on EBS volumes for as long as you want
• MSK Serverless
• Run Apache Kafka on MSK without managing the capacity
• MSK automatically provisions resources and scales compute & storage

Kinesis Data Streams vs. Amazon MSK

| Kinesis Data Streams        | Amazon MSK                                     |
| --------------------------- | ---------------------------------------------- |
| 1 MB message size limit     | • 1MB default, configure for higher (ex: 10MB) |
| • Data Streams with Shards  | Kafka Topics with Partitions                   |
| • Shard Splitting & Merging | Can only add partitions to a topic             |
| • KMS at-rest encryption    | KMS at-rest encryption                         |
| TLS In-flight encryption    | PLAINTEXT or TLS In-flight Encryption          |
## Consumers 
- Kinesis Data Analytics for Apache Flink
- AWS Glue Streaming ETL Jobs Powered by Apache Spark Streaming
- Lambda
- Applications Running on:
	- Amazon EC2
	- ECS
	- EKS
