# Athena
- **Serverless** query service to analyze data stored in Amazon S3
- Uses standard SQL language to query the files (built on Presto)
- Supports CSV, JSON, ORC, Avro, and Parquet
- Pricing: $5.00 per TB of data scanned
- Commonly used with Amazon Quicksight for reporting/dashboards
- Use cases: Business intelligence / analytics / reporting, analyze & query VPC Flow Logs, ELB Logs, CloudTrail trails, etc
- **Exam Tip**: analyze data in S3 using serverless SQL, use Athena

## Performance Improvement
- Use columnar data for cost-savings (less scan)
- Apache Parquet or ORC is recommended
- Huge performance improvement
- Use Glue to convert your data to Parquet or ORC
- Compress data for smaller retrievals (bzip2, gzip, lz4, snappy, zlip, zstd…)
- Partition datasets in S3 for easy querying on virtual columns
```
s3://yourBucket/pathToTable
/<PARTITION_COLUMN_NAME>=<VALUE>
/<PARTITION_COLUMN_NAME>=<VALUE>
/<PARTITION_COLUMN_NAME>=<VALUE>
```
- Example: `s3://athena-examples/flight/parquet/year=1991/month=1/day=1/` 
- Use larger files (> 128 MB) to minimize overhead

## Federated Query
- Allows you to run SQL queries across data stored in relational, non-relational, object, and custom data sources (AWS or on-premises)
- Uses Data Source Connectors that run on AWS Lambda to run Federated Queries (e.g., CloudWatch Logs, DynamoDB, RDS, …)
- Store the results back in Amazon S3

# Redshift
Amazon Redshift is built on cloud economics that scale with your usage - powering modern analytics and autonomous agentic AI workloads on your data warehouse. Redshift delivers up to 2.2x better price-performance and 7x better throughput than other cloud data warehouses. Redshift’s new Graviton-based RG instances run data warehouse and data lake workloads up to 2.4x as fast as previous generation RA3 instances at 30% lower price per vCPU and includes an integrated data lake query engine. Redshift Serverless helps you go from data to insights in seconds without managing infrastructure. Redshift powers SQL analytics on unified data across your lakehouse in Amazon SageMaker. Zero-ETL integrations enable near real-time analytics by connecting streaming services, operational databases, and third-party enterprise applications without complex data pipelines. Use Redshift as your structured knowledge base in Amazon Bedrock for more accurate generative AI output.

• Redshift is based on PostgreSQL, but it’s not used for OLTP
• It’s OLAP – online analytical processing (analytics and data warehousing)
• 10x better performance than other data warehouses, scale to PBs of data
• Columnar storage of data (instead of row based) & parallel query engine
• Two modes: Provisioned cluster or Serverless cluster
• Has a SQL interface for performing the queries
• BI tools such as Amazon Quicksight or Tableau integrate with it
• vs Athena: faster queries / joins / aggregations thanks to indexes

## Redshift Spectrum
Using Amazon Redshift Spectrum, you can efficiently query and retrieve structured and semi-structured data from files in Amazon S3 without having to load the data into Amazon Redshift tables. Redshift Spectrum queries employ massive parallelism to run very fast against large datasets. Much of the processing occurs in the Redshift Spectrum layer, and most of the data remains in Amazon S3. Multiple clusters can concurrently query the same dataset in Amazon S3 without the need to make copies of the data for each cluster.

Redshift cluster
• Leader node: for query
planning, results
aggregation
• Compute node: for
performing the queries,
send results to leader
• Provisioned mode:
• Choose instance types
in advance
• Can reserve instances
for cost savings


• Redshift has “Multi-AZ” mode for some clusters
• Snapshots are point-in-time backups of a cluster,
stored internally in S3
• Snapshots are incremental (only what has
changed is saved)
• You can restore a snapshot into a new cluster
• Automated: every 8 hours, every 5 GB, or on a
schedule. Set retention between 1 to 35 days
• Manual: snapshot is retained until you delete it

• You can configure Amazon Redshift to
automatically copy snapshots (automated or
manual) of a cluster to another AWS Region


# Amazon OpenSearch
• Amazon OpenSearch is successor to Amazon ElasticSearch
• In DynamoDB, queries only exist by primary key or indexes…
• With OpenSearch, you can search any field, even partially matches
• It’s common to use OpenSearch as a complement to another database
• Two modes: managed cluster or serverless cluster
• Does not natively support SQL (can be enabled via a plugin)
• Ingestion from Kinesis Data Firehose, AWS IoT, and CloudWatch Logs
• Security through Cognito & IAM, KMS encryption, TLS
• Comes with OpenSearch Dashboards (visualization)

