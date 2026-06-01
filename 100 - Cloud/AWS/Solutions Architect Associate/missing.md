# AWS SAA-C03: Missing Topics

Topics not yet studied. Fill in when studied (and move to their own note).

Exam: 65 questions, 130 minutes, passing score 720/1000.

## Domain 1: Design Secure Architectures (30%)

### IAM & Access Management

- [ ] IAM users, groups, roles, policies (inline vs managed)
- [ ] IAM policy evaluation logic (allow/deny, SCPs, resource policies)
- [ ] Cross-account access (STS, AssumeRole, resource-based policies)
- [ ] IAM Identity Center (formerly AWS SSO)
- [ ] Federation (SAML 2.0, OIDC, Cognito identity pools)
- [ ] MFA enforcement, root user best practices
- [ ] AWS Organizations, SCPs, Control Tower (multi-account governance)
- [ ] Resource policies (S3 bucket policies, KMS key policies)
- [ ] AWS Directory Service federation with IAM roles

### Networking Security

- [ ] VPC architecture (public/private subnets, route tables, internet gateway)
- [ ] Security groups vs network ACLs (stateful vs stateless)
- [ ] NAT gateways vs NAT instances
- [ ] AWS Network Firewall
- [ ] VPC endpoints (Gateway vs Interface/PrivateLink)
- [ ] VPN (Site-to-Site, Client VPN) vs Direct Connect
- [ ] AWS Transit Gateway
- [ ] Network segmentation strategies

### Application Security

- [ ] AWS WAF (web ACLs, rules, rate limiting)
- [ ] AWS Shield (Standard vs Advanced, DDoS protection)
- [ ] Amazon GuardDuty (threat detection)
- [ ] Amazon Macie (data classification, PII discovery)
- [ ] Amazon Inspector (vulnerability scanning)
- [ ] AWS Security Hub (security posture aggregation)
- [ ] Amazon Detective (incident investigation)
- [ ] AWS Firewall Manager (centralized WAF/Shield management)

### Data Security & Encryption

- [ ] AWS KMS (key types, key policies, key rotation, grants)
- [ ] AWS CloudHSM (dedicated HSM)
- [ ] AWS Certificate Manager (ACM, TLS certificates)
- [ ] Encryption at rest (S3, EBS, RDS, DynamoDB)
- [ ] Encryption in transit (TLS, ACM)
- [ ] AWS Secrets Manager (rotation, integration)
- [ ] Data lifecycle, classification, retention policies
- [ ] S3 Object Lock, Glacier Vault Lock

### Compliance & Governance

- [ ] Shared responsibility model (SAA depth)
- [ ] AWS Artifact (compliance reports)
- [ ] AWS Audit Manager
- [ ] AWS Config (resource compliance tracking)
- [ ] Amazon Cognito (user pools vs identity pools)

## Domain 2: Design Resilient Architectures (26%)

### Scalable & Loosely Coupled Architectures

- [ ] Amazon SQS (standard vs FIFO, visibility timeout, dead-letter queues)
- [ ] Amazon SNS (pub/sub, fan-out, message filtering)
- [ ] Amazon EventBridge (event buses, rules, schemas)
- [ ] Amazon MQ (managed message broker)
- [ ] AWS Step Functions (state machines, workflow orchestration)
- [ ] AWS AppSync (GraphQL, real-time subscriptions)
- [ ] Amazon API Gateway (REST API, stages, throttling, caching)
- [ ] Microservices design principles (stateless vs stateful)
- [ ] Event-driven architecture patterns
- [ ] Horizontal vs vertical scaling
- [ ] Multi-tier architecture design
- [ ] Caching strategies (CloudFront, ElastiCache, API Gateway caching)
- [ ] Edge accelerators (CDN, CloudFront)

### High Availability & Fault Tolerance

- [ ] Disaster recovery strategies (backup/restore, pilot light, warm standby, active-active)
- [ ] RPO and RTO concepts
- [ ] Route 53 routing policies (simple, weighted, latency, failover, geolocation, multivalue)
- [ ] Route 53 health checks
- [ ] Elastic Load Balancing (ALB vs NLB vs GWLB)
- [ ] Auto Scaling (target tracking, step, simple, scheduled policies)
- [ ] Multi-AZ deployments (RDS, ElastiCache, EFS)
- [ ] Multi-Region architectures
- [ ] Failover strategies
- [ ] Immutable infrastructure patterns
- [ ] Service quotas and throttling
- [ ] AWS X-Ray (distributed tracing, workload visibility)
- [ ] RDS Proxy (connection pooling)

## Domain 3: Design High-Performing Architectures (24%)

### Storage Performance

- [ ] Amazon S3 (storage classes, performance optimization, transfer acceleration)
- [ ] Amazon EFS (performance modes, throughput modes, storage classes)
- [ ] Amazon FSx (Lustre, Windows, NetApp ONTAP, OpenZFS)
- [ ] S3 Transfer Acceleration vs CloudFront
- [ ] Hybrid storage (Storage Gateway, DataSync)
- [ ] Storage type selection (object vs file vs block)

### Compute Performance

- [ ] EC2 instance families and selection (compute/memory/storage/GPU optimized)
- [ ] EC2 Auto Scaling (scaling policies, launch templates, mixed instances)
- [ ] AWS Lambda (memory sizing, cold starts, concurrency, layers)
- [ ] AWS Fargate (serverless containers)
- [ ] Amazon ECS vs EKS (container orchestration)
- [ ] AWS Batch (batch processing workloads)
- [ ] Amazon EMR (big data processing)
- [ ] AWS Outposts (hybrid compute)
- [ ] AWS Wavelength (edge compute for 5G)

### Database Performance

- [ ] Amazon RDS (engines, Multi-AZ, read replicas, instance selection)
- [ ] Amazon Aurora (architecture, Aurora Serverless, Global Database)
- [ ] Amazon DynamoDB (partition keys, sort keys, GSIs, LSIs, DAX)
- [ ] Amazon ElastiCache (Redis vs Memcached, clustering, replication)
- [ ] Amazon Redshift (data warehousing, distribution styles)
- [ ] Amazon Neptune (graph database)
- [ ] Amazon DocumentDB (MongoDB-compatible)
- [ ] Amazon Keyspaces (Cassandra-compatible)
- [ ] Database migration (DMS, SCT, homogeneous vs heterogeneous)
- [ ] Read replicas and caching strategies

### Network Performance

- [ ] Amazon CloudFront (distributions, origins, behaviors, Lambda@Edge)
- [ ] AWS Global Accelerator (anycast, static IPs)
- [ ] CloudFront vs Global Accelerator
- [ ] VPC design for performance (subnet tiers, IP planning)
- [ ] Load balancer selection (ALB vs NLB vs GWLB)
- [ ] AWS PrivateLink
- [ ] Direct Connect (port speeds, Link Aggregation Groups)

### Data Ingestion & Transformation

- [ ] Amazon Kinesis (Data Streams, Data Firehose, Video Streams)
- [ ] Amazon MSK (Managed Streaming for Kafka)
- [ ] AWS Glue (ETL, Data Catalog, crawlers)
- [ ] Amazon Athena (serverless SQL on S3)
- [ ] AWS Lake Formation (data lake governance)
- [ ] AWS DataSync (data transfer)
- [ ] AWS Transfer Family (SFTP, FTPS, FTP)
- [ ] Amazon OpenSearch Service (search and analytics)

## Domain 4: Design Cost-Optimized Architectures (20%)

### Storage Cost Optimization

- [ ] S3 storage classes and lifecycle policies (Standard, IA, One Zone-IA, Glacier, Glacier Deep Archive)
- [ ] S3 Intelligent-Tiering
- [ ] S3 Requester Pays
- [ ] EBS volume type selection and sizing
- [ ] EBS snapshot cost management
- [ ] EFS storage classes and lifecycle
- [ ] FSx cost considerations
- [ ] Backup strategies and cost trade-offs
- [ ] Data transfer costs (ingress/egress, cross-region, internet)

### Compute Cost Optimization

- [ ] EC2 pricing models (On-Demand, Reserved, Spot, Savings Plans, Dedicated)
- [ ] Spot Instances (Spot Fleet, interruption handling)
- [ ] Reserved Instances (Standard vs Convertible, regional vs zonal)
- [ ] Savings Plans (Compute vs EC2 Instance vs SageMaker)
- [ ] Instance right-sizing (Compute Optimizer)
- [ ] Auto Scaling for cost (scheduled scaling, predictive scaling)
- [ ] EC2 hibernation for cost savings
- [ ] Lambda cost optimization (memory vs duration trade-off)
- [ ] Fargate vs EC2 for container costs
- [ ] AWS Budgets (alerts, actions)
- [ ] AWS Cost Explorer (analysis, forecasting)
- [ ] AWS Cost and Usage Report
- [ ] Cost allocation tags

### Database Cost Optimization

- [ ] RDS pricing (On-Demand vs Reserved, Aurora pricing)
- [ ] DynamoDB capacity modes (on-demand vs provisioned, reserved capacity)
- [ ] DynamoDB DAX cost trade-offs
- [ ] ElastiCache node selection and reserved nodes
- [ ] Redshift pricing (on-demand vs reserved, Spectrum, concurrency scaling)
- [ ] Database backup retention cost impact
- [ ] Serverless database options (Aurora Serverless, DynamoDB)

### Network Cost Optimization

- [ ] NAT gateway costs (per-AZ vs shared, data processing charges)
- [ ] VPC endpoints to reduce data transfer costs
- [ ] Direct Connect vs VPN vs internet cost comparison
- [ ] Transit Gateway costs (attachment hours, data processing)
- [ ] CloudFront pricing and caching to reduce origin costs
- [ ] Data transfer cost minimization (same-AZ, private IPs)
- [ ] Bandwidth allocation planning

## Management & Governance

- [ ] AWS CloudFormation (stacks, templates, drift detection, nested stacks)
- [ ] AWS Systems Manager (Parameter Store, Session Manager, Run Command, Patch Manager)
- [ ] Amazon CloudWatch (metrics, alarms, logs, dashboards, CloudWatch Events)
- [ ] AWS CloudTrail (API auditing, multi-region trails)
- [ ] AWS Trusted Advisor (checks across cost, performance, security, fault tolerance)
- [ ] AWS Well-Architected Tool (workload reviews)
- [ ] AWS Health Dashboard (service health, scheduled changes)
- [ ] AWS Service Catalog
- [ ] AWS License Manager
- [ ] AWS Resource Access Manager (RAM)
- [ ] AWS CLI essentials

## Migration & Transfer

- [ ] AWS Application Migration Service (MGN)
- [ ] AWS DMS (Database Migration Service, SCT)
- [ ] AWS Snow Family (Snowcone, Snowball, Snowmobile)
- [ ] AWS DataSync
- [ ] AWS Transfer Family
- [ ] Migration strategies (6 Rs: rehost, replatform, repurchase, refactor, retire, retain)

## ML & AI Services (awareness level)

- [ ] Amazon SageMaker AI
- [ ] Amazon Rekognition (image/video analysis)
- [ ] Amazon Polly (text-to-speech)
- [ ] Amazon Transcribe (speech-to-text)
- [ ] Amazon Translate (translation)
- [ ] Amazon Comprehend (NLP)
- [ ] Amazon Lex (chatbots)
- [ ] Amazon Kendra (enterprise search)
- [ ] Amazon Textract (document extraction)

## Other In-Scope Services

- [ ] AWS Amplify (front-end web/mobile)
- [ ] AWS Elastic Beanstalk (managed app platform)
- [ ] AWS Serverless Application Repository
- [ ] Amazon AppFlow (SaaS data integration)
- [ ] AWS Device Farm (mobile testing)
- [ ] Amazon Elastic Transcoder (media transcoding)
- [ ] Amazon Kinesis Video Streams
- [ ] Amazon Managed Grafana
- [ ] Amazon Managed Service for Prometheus
- [ ] AWS Compute Optimizer
- [ ] Amazon QuickSight (BI visualization)
- [ ] AWS Data Exchange

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
