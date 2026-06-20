---
domain: aws
track: solutions-architect-associate
topic: storage
type: note
tags:
  - aws
  - solutions-architect-associate
  - storage
  - s3
  - event-notifications
  - s3-performance
  - storage-lens
---

# S3 Operations

> For core S3 concepts (buckets, versioning, replication, storage classes, lifecycle), see [[100 - Cloud/AWS/Solutions Architect Associate/S3|S3]].

## Event Notifications

S3 can notify downstream services when objects are created, deleted, restored, or replicated (`S3:ObjectCreated`, `S3:ObjectRemoved`, `S3:ObjectRestore`, `S3:Replication`).

- Object name filtering supported (e.g. `*.jpg`)
- Use case: generate thumbnails of images uploaded to S3
- Typically deliver in seconds, sometimes up to a minute
- Can create as many event destinations as desired

**Destinations:** SNS, SQS, Lambda. The resource policy is attached to the **destination** (SNS/SQS/Lambda resource policy), not on S3.

### With EventBridge
- Advanced filtering with JSON rules (metadata, object size, name)
- Multiple destinations (Step Functions, Kinesis Streams/Firehose)
- EventBridge capabilities: archive, replay events, reliable delivery

## Performance

### Baseline
- S3 automatically scales to high request rates, latency 100-200 ms
- At least **3,500 PUT/COPY/POST/DELETE** or **5,500 GET/HEAD** requests per second **per prefix**
- No limit on number of prefixes in a bucket
- Example: `bucket/folder1/sub1/file` has prefix `/folder1/sub1/`
- Spread across 4 prefixes evenly = 22,000 GET/HEAD requests per second

### Multi-Part Upload
- Recommended for files > 100 MB, **required** for files > 5 GB
- Parallelizes uploads to speed up transfers

### Transfer Acceleration

Increases transfer speed by routing file to an AWS edge location, which forwards data to the target S3 bucket region. Compatible with multi-part upload.

### S3 Transfer Acceleration vs CloudFront

| Aspect | S3 Transfer Acceleration | CloudFront |
|--------|-------------------------|------------|
| **Purpose** | Accelerate uploads TO S3 | Distribute content FROM origins (S3, VPC, Custom) |
| **Direction** | Upload-focused | Download-focused (caches at edge) |
| **Mechanism** | Edge location receives upload, forwards to S3 bucket | Edge location caches content, serves from cache |
| **Caching** | No caching; forwards to S3 | Cached for TTL (e.g. a day) |
| **Best for** | Large file uploads from distant locations | Static content available everywhere, repeated reads |
| **Access** | Read and write | Read only (cached content) |
| **Scope** | Routes to specific S3 bucket region | Global edge network (Points of Presence) |
| **Integrations** | S3 multi-part upload | S3, EC2, ELB, Route 53, WAF, Shield |

Both use AWS edge locations but serve different purposes: Transfer Acceleration speeds up uploads to S3, CloudFront speeds up downloads by caching content globally.

### Byte-Range Fetches
- Parallelize GETs by requesting specific byte ranges
- Better resilience in case of failures
- Retrieve only partial data (e.g. head of a file)
- Speed up downloads

## Batch Operations

Perform bulk operations on existing S3 objects with a single request:

- Modify object metadata and properties
- Copy objects between buckets
- Encrypt un-encrypted objects
- Modify ACLs, tags
- Restore objects from Glacier
- Invoke Lambda function for custom action on each object

A job consists of a list of objects, the action to perform, and optional parameters. S3 Batch Operations manages retries, tracks progress, sends completion notifications, and generates reports.

Use **S3 Inventory** to get the object list, and **Athena** to query and filter objects.

## Storage Lens

Understand, analyze, and optimize storage across the entire AWS Organization. Discover anomalies, identify cost efficiencies, and apply data protection best practices (30 days of usage and activity metrics by default).

- Aggregate data for Organization, specific accounts, regions, buckets, or prefixes
- Default dashboard or create custom dashboards
- Export metrics daily to an S3 bucket (CSV or Parquet format)

### Default Dashboard
- Visualize summarized insights and trends for both free and advanced metrics
- Shows Multi-Region and Multi-Account data
- Preconfigured by Amazon S3; cannot be deleted, but can be disabled

### Metrics Categories

| Category | Key Metrics | Use Cases |
|----------|-------------|-----------|
| **Summary** | StorageBytes, ObjectCount | Identify fastest-growing or unused buckets/prefixes |
| **Cost-Optimization** | NonCurrentVersionStorageBytes, IncompleteMultipartUploadStorageBytes | Find buckets with incomplete multipart uploads older than 7 days; identify objects that could transition to lower-cost storage classes |
| **Data-Protection** | VersioningEnabledBucketCount, MFADeleteEnabledBucketCount, SEKMSEnabledBucketCount, CrossRegionReplicationRuleCount | Identify buckets not following data-protection best practices |
| **Access-Management** | ObjectOwnershipBucketOwnerEnforcedBucketCount | Identify which Object Ownership settings buckets use |
| **Event** | EventNotificationEnabledBucketCount | Identify which buckets have S3 Event Notifications configured |
| **Performance** | TransferAccelerationEnabledBucketCount | Identify which buckets have Transfer Acceleration enabled |
| **Activity** | AllRequests, GetRequests, PutRequests, ListRequests, BytesDownloaded | Understand how storage is requested |
| **Detailed Status Code** | 200OKStatusCount, 403ForbiddenErrorCount, 404NotFoundErrorCount | HTTP status code insights |

### Free vs. Paid

| | Free | Advanced (Paid) |
|---|---|---|
| **Metrics** | ~28 usage metrics (Summary) | Activity, Advanced Cost Optimization, Advanced Data Protection, Status Code |
| **Data retention** | 14 days | 15 months |
| **Features** | Default dashboards | CloudWatch Publishing, Prefix Aggregation, Recommendations |
| **Availability** | Automatic for all customers | Opt-in |

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
