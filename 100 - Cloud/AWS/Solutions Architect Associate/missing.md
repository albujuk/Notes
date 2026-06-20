# AWS SAA-C03: Missing Topics

Topics not yet studied. Fill in when studied (and move to their own note).

Exam: 65 questions, 130 minutes, passing score 720/1000.

## Domain 1: Design Secure Architectures (30%)

### IAM & Access Management

- [x] IAM users, groups, roles, policies (inline vs managed) → [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS IAM (Identity and Access Management)|Security: IAM]]
- [ ] Cross-account access (STS, AssumeRole, resource-based policies)
- [ ] Federation (SAML 2.0, OIDC, Cognito identity pools)
- [ ] MFA enforcement, root user best practices

### Networking Security

- [ ] Network segmentation strategies

### Application Security

- [x] AWS WAF (web ACLs, rules, rate limiting) → [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS WAF (Web Application Firewall)|Security: WAF]] / [[100 - Cloud/AWS/Solutions Architect Associate/WAF Shield Firewall Manager|SAA: WAF]]
- [x] AWS Shield (Standard vs Advanced, DDoS protection) → [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Shield|Security: Shield]] / [[100 - Cloud/AWS/Solutions Architect Associate/WAF Shield Firewall Manager|SAA: Shield]]
- [x] Amazon GuardDuty (threat detection) → [[100 - Cloud/AWS/Cloud Practitioner/Security#Amazon GuardDuty|Security: GuardDuty]] / [[100 - Cloud/AWS/Solutions Architect Associate/Threat Detection|SAA: GuardDuty]]
- [x] Amazon Macie (data classification, PII discovery) → [[100 - Cloud/AWS/Cloud Practitioner/Security#Amazon Macie|Security: Macie]] / [[100 - Cloud/AWS/Solutions Architect Associate/Threat Detection|SAA: Macie]]
- [x] Amazon Inspector (vulnerability scanning) → [[100 - Cloud/AWS/Cloud Practitioner/Security#Amazon Inspector|Security: Inspector]] / [[100 - Cloud/AWS/Solutions Architect Associate/Threat Detection|SAA: Inspector]]
- [x] AWS Security Hub (security posture aggregation) → [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Security Hub|Security: Security Hub]]
- [x] Amazon Detective (incident investigation) → [[100 - Cloud/AWS/Cloud Practitioner/Security#Amazon Detective|Security: Detective]]
- [x] AWS Firewall Manager (centralized WAF/Shield management) → [[100 - Cloud/AWS/Solutions Architect Associate/WAF Shield Firewall Manager|SAA: Firewall Manager]]

### Data Security & Encryption

- [x] Encryption at rest (S3, EBS, RDS, DynamoDB) → [[100 - Cloud/AWS/Cloud Practitioner/Security#Data Protection|Security: Data Protection]] / [[100 - Cloud/AWS/Cloud Practitioner/S3#Security|S3: Security]] / [[100 - Cloud/AWS/Cloud Practitioner/Database|Database]]
- [x] Encryption in transit (TLS, ACM) → [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Certificate Manager (ACM)|Security: ACM]]
- [x] Data lifecycle, classification, retention policies → [[100 - Cloud/AWS/Cloud Practitioner/S3#Object Lifecycle Management|S3: Lifecycle]]
- [ ] AWS Certificate Manager integration with S3 (custom domains)

### Compliance & Governance

- [x] Shared responsibility model (SAA depth) → [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Shared Responsibility Model|Security: Shared Responsibility]]
- [x] AWS Artifact (compliance reports) → [[100 - Cloud/AWS/Cloud Practitioner/Governance#AWS Artifact|Governance: Artifact]]
- [x] AWS Audit Manager → [[100 - Cloud/AWS/Cloud Practitioner/Governance#AWS Audit Manager|Governance: Audit Manager]]
- [ ] Amazon Cognito (user pools vs identity pools)

## Domain 2: Design Resilient Architectures (26%)

### Scalable & Loosely Coupled Architectures

- [x] Amazon SQS (standard vs FIFO, visibility timeout, dead-letter queues) → [[100 - Cloud/AWS/Cloud Practitioner/Messaging#Amazon SQS (Simple Queue Service)|Messaging: SQS]] / [[100 - Cloud/AWS/Solutions Architect Associate/Messaging#Amazon SQS: Standard Queue|SAA: SQS Standard]]
- [x] Amazon SNS (pub/sub, fan-out, message filtering) → [[100 - Cloud/AWS/Cloud Practitioner/Messaging#Amazon SNS (Simple Notification Service)|Messaging: SNS]]
- [x] Amazon EventBridge (event buses, rules, schemas) → [[100 - Cloud/AWS/Cloud Practitioner/Messaging#Amazon EventBridge|Messaging: EventBridge]]
- [ ] Amazon MQ (managed message broker)
- [ ] AWS Step Functions (state machines, workflow orchestration)
- [x] AWS AppSync (GraphQL, real-time subscriptions) → [[100 - Cloud/AWS/Cloud Practitioner/SpecializedServices#Development Services|Specialized: Dev Services]]
- [x] Amazon API Gateway (REST API, stages, throttling, caching, endpoint types) → [[100 - Cloud/AWS/Cloud Practitioner/Networking#API Gateway|Networking: API Gateway]] / [[100 - Cloud/AWS/Solutions Architect Associate/API Gateway|SAA: API Gateway]]
- [ ] Microservices design principles (stateless vs stateful)
- [x] Event-driven architecture patterns → [[100 - Cloud/AWS/Cloud Practitioner/Messaging|Messaging]]
- [x] Horizontal vs vertical scaling → [[100 - Cloud/AWS/Cloud Practitioner/Compute#Elasticity (Scaling)|Compute: Scaling]]
- [ ] Multi-tier architecture design
- [x] Caching strategies (CloudFront, ElastiCache, API Gateway caching) → [[100 - Cloud/AWS/Cloud Practitioner/Networking#CDN: CloudFront|Networking: CloudFront]] / [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon ElastiCache|Database: ElastiCache]] / [[100 - Cloud/AWS/Cloud Practitioner/Networking#API Gateway|Networking: API Gateway]]
- [x] Edge accelerators (CDN, CloudFront) → [[100 - Cloud/AWS/Cloud Practitioner/Networking#CDN: CloudFront|Networking: CloudFront]]

### High Availability & Fault Tolerance

- [ ] Disaster recovery strategies (backup/restore, pilot light, warm standby, active-active)
- [ ] RPO and RTO concepts
- [ ] Multi-Region architectures
- [ ] Failover strategies
- [ ] Immutable infrastructure patterns
- [ ] Service quotas and throttling
- [x] AWS X-Ray (distributed tracing, workload visibility) → [[100 - Cloud/AWS/Cloud Practitioner/SpecializedServices#Development Services|Specialized: Dev Services]]

## Domain 3: Design High-Performing Architectures (24%)

### Storage Performance

- [x] Storage type selection (object vs file vs block) → [[100 - Cloud/AWS/Cloud Practitioner/Storage|Storage]]

### Compute Performance

- [ ] EC2 instance families and selection (compute/memory/storage/GPU optimized)
- [x] AWS Fargate (serverless containers) → [[100 - Cloud/AWS/Cloud Practitioner/Containers#AWS Fargate|Containers: Fargate]]
- [x] Amazon ECS vs EKS (container orchestration) → [[100 - Cloud/AWS/Cloud Practitioner/Containers|Containers]]
- [ ] Amazon ECS Anywhere (run ECS tasks on-premises or in other clouds)
- [ ] Amazon EKS Anywhere (run EKS clusters on-premises or in other clouds)
- [ ] Amazon EKS Distro (open-source Kubernetes distribution used by EKS Anywhere)
- [x] AWS Batch (batch processing workloads) → [[100 - Cloud/AWS/Cloud Practitioner/Compute#AWS Batch|Compute: Batch]]
- [x] AWS Outposts (hybrid compute) → [[100 - Cloud/AWS/Cloud Practitioner/Compute#AWS Outposts|Compute: Outposts]] / [[100 - Cloud/AWS/Cloud Practitioner/Global Infrastructure#Outposts|Global Infra: Outposts]]
- [ ] AWS Wavelength (edge compute for 5G)
- [ ] VMware Cloud on AWS (hybrid cloud with VMware)

### Database Performance

- [x] Amazon RDS (engines, Multi-AZ, read replicas, instance selection) → [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon RDS (Relational Database Service)|Database: RDS]]
- [ ] Aurora Serverless v1 vs v2, ACU scaling, pause/resume, write forwarding
- [ ] Aurora parallel query, multi-master, I/O-Optimized
- [x] Amazon DynamoDB (partition keys, sort keys, GSIs, LSIs, DAX) → [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon DynamoDB|Database: DynamoDB]]
- [x] Amazon Neptune (graph database) → [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon Neptune|Database: Neptune]]
- [x] Amazon DocumentDB (MongoDB-compatible) → [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon DocumentDB|Database: DocumentDB]]
- [ ] Amazon Keyspaces (Cassandra-compatible)
- [x] Database migration (DMS, SCT, homogeneous vs heterogeneous) → [[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#Migration Services|CAF: Migration]]

### Network Performance

### Data Ingestion & Transformation

- [x] Amazon Kinesis (Data Streams, Data Firehose, Video Streams) → [[100 - Cloud/AWS/Cloud Practitioner/Analytics#Data Ingestion|Analytics: Ingestion]]
- [ ] Amazon MSK (Managed Streaming for Kafka)
- [x] AWS Glue (ETL, Data Catalog, crawlers) → [[100 - Cloud/AWS/Cloud Practitioner/Analytics#AWS Glue|Analytics: Glue]] / [[100 - Cloud/AWS/Cloud Practitioner/Analytics#AWS Glue Data Catalog|Analytics: Glue Catalog]] / [[100 - Cloud/AWS/Solutions Architect Associate/Analytics#AWS Glue|SAA: Glue]]

## Domain 4: Design Cost-Optimized Architectures (20%)

### Storage Cost Optimization

- [x] EBS snapshot cost management → [[100 - Cloud/AWS/Cloud Practitioner/BlockStorage#EBS Snapshots|BlockStorage: Snapshots]]
- [x] EFS storage classes and lifecycle → [[100 - Cloud/AWS/Cloud Practitioner/FileStorage#EFS Storage Classes|FileStorage: EFS Classes]]
- [ ] FSx cost considerations
- [ ] Backup strategies and cost trade-offs
- [ ] AWS Backup (centralized backup management across AWS services)

### Compute Cost Optimization

- [ ] EC2 pricing models (On-Demand, Reserved, Spot, Savings Plans, Dedicated)
- [ ] Spot Instances (Spot Fleet, interruption handling)
- [ ] Reserved Instances (Standard vs Convertible, regional vs zonal)
- [ ] Savings Plans (Compute vs EC2 Instance vs SageMaker)
- [ ] Instance right-sizing (Compute Optimizer)
- [x] Auto Scaling for cost (scheduled scaling, predictive scaling) → [[100 - Cloud/AWS/Cloud Practitioner/Compute#Auto Scaling|Compute: Auto Scaling]]
- [x] Lambda cost optimization (memory vs duration trade-off) → [[100 - Cloud/AWS/Cloud Practitioner/Compute#AWS Lambda|Compute: Lambda]]
- [x] Fargate vs EC2 for container costs → [[100 - Cloud/AWS/Cloud Practitioner/Containers#AWS Fargate|Containers: Fargate]]
- [x] AWS Budgets (alerts, actions) → [[100 - Cloud/AWS/Cloud Practitioner/Billing#AWS Budgets|Billing: Budgets]]
- [x] AWS Cost Explorer (analysis, forecasting) → [[100 - Cloud/AWS/Cloud Practitioner/Billing#AWS Cost Explorer|Billing: Cost Explorer]]
- [ ] AWS Cost and Usage Report
- [ ] Cost allocation tags

### Database Cost Optimization

- [ ] RDS pricing (On-Demand vs Reserved, Aurora pricing)
- [ ] DynamoDB capacity modes (on-demand vs provisioned, reserved capacity)
- [ ] DynamoDB DAX cost trade-offs
- [ ] ElastiCache node selection and reserved nodes
- [ ] Redshift pricing (on-demand vs reserved, Spectrum, concurrency scaling)
- [ ] Database backup retention cost impact
- [x] Serverless database options (Aurora Serverless, DynamoDB) → [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon Aurora|Database: Aurora]] / [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon DynamoDB|Database: DynamoDB]]

### Network Cost Optimization

- [ ] Transit Gateway costs (attachment hours, data processing)
- [ ] CloudFront pricing and caching to reduce origin costs
- [ ] Bandwidth allocation planning

## Management & Governance

- [x] AWS CloudFormation (stacks, templates, drift detection, nested stacks) → [[100 - Cloud/AWS/Cloud Practitioner/CloudFormation|CloudFormation]]
- [x] AWS Systems Manager (Parameter Store, Session Manager, Run Command, Patch Manager) → [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Systems Manager|Security: Systems Manager]]
- [x] AWS Trusted Advisor (checks across cost, performance, security, fault tolerance) → [[100 - Cloud/AWS/Cloud Practitioner/Monitoring#AWS Trusted Advisor|Monitoring: Trusted Advisor]]
- [x] AWS Well-Architected Tool (workload reviews) → [[100 - Cloud/AWS/Cloud Practitioner/WellArchitected#AWS Well-Architected Tool|WellArchitected: Tool]]
- [x] AWS Health Dashboard (service health, scheduled changes) → [[100 - Cloud/AWS/Cloud Practitioner/Monitoring#AWS Health|Monitoring: Health]]
- [x] AWS Service Catalog → [[100 - Cloud/AWS/Cloud Practitioner/Governance#AWS Service Catalog|Governance: Service Catalog]]
- [x] AWS License Manager → [[100 - Cloud/AWS/Cloud Practitioner/Governance#AWS License Manager|Governance: License Manager]]
- [ ] AWS Resource Access Manager (RAM)
- [ ] AWS CLI essentials

## Migration & Transfer

- [x] AWS Application Migration Service (MGN) → [[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#Migration Services|CAF: Migration]]
- [x] AWS DMS (Database Migration Service, SCT) → [[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#Migration Services|CAF: Migration]]
- [x] Migration strategies (6 Rs: rehost, replatform, repurchase, refactor, retire, retain) → [[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#7 Rs of Migration|CAF: 7 Rs]]

## Other In-Scope Services

- [x] AWS Amplify (front-end web/mobile) → [[100 - Cloud/AWS/Cloud Practitioner/SpecializedServices#Development Services|Specialized: Dev Services]]
- [ ] AWS Serverless Application Repository
- [ ] Amazon AppFlow (SaaS data integration)
- [ ] AWS Device Farm (mobile testing)
- [ ] Amazon Elastic Transcoder (media transcoding)
- [ ] Amazon Kinesis Video Streams
- [ ] Amazon Managed Grafana
- [ ] Amazon Managed Service for Prometheus
- [ ] AWS Compute Optimizer
- [ ] AWS Data Exchange

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
