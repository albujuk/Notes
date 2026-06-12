# AWS SAA-C03: Depth Gaps Beyond Cloud Practitioner

Many items in [[missing|missing.md]] are checked because they were covered at Cloud Practitioner awareness level. SAA-C03 requires **deep, scenario-based knowledge**. This file tracks the specific sub-topics needed for SAA depth that are NOT covered by the CCP notes.

---

## Domain 1: Design Secure Architectures (30%)

### IAM & Access Management

**[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS IAM (Identity and Access Management)|Security: IAM]]** → SAA needs:
- IAM policy evaluation logic (explicit deny always wins, order of evaluation)
- Condition keys (aws:SourceIp, aws:PrincipalArn, aws:RequestedRegion)
- Policy types: identity-based vs resource-based vs permissions boundaries vs SCPs
- IAM role chaining and session tags
- Temporary credentials duration
- IAM Access Analyzer for resource policies
- Permissions boundaries for delegating permissions without granting them

**[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS IAM Identity Center|Security: IAM Identity Center]]** → SAA needs:
- Permission sets and inline vs managed policies within Identity Center
- Attribute-based access control (ABAC) with IAM Identity Center
- Integration with AWS Organizations for multi-account access
- SCIM provisioning from external IdP
- SSO assignment to accounts vs OUs

**[[100 - Cloud/AWS/Cloud Practitioner/Governance#AWS Organizations|Governance: Organizations]]** / **[[100 - Cloud/AWS/Cloud Practitioner/Governance#AWS Control Tower|Control Tower]]** → SAA needs:
- SCP attachment points and inheritance (root vs OU vs account)
- SCP effect on service-linked roles
- Tag policies and data protection policies
- Control Tower guardrails (preventive vs detective)
- Account vending machine patterns
- Multi-account network design (shared services VPC vs distributed)

**[[100 - Cloud/AWS/Cloud Practitioner/S3#Security|S3: Security]]** / **[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS KMS (Key Management Service)|Security: KMS]]** → SAA needs:
- S3 bucket policy condition keys (aws:SourceVpce, aws:SourceVpc, aws:Referer)
- VPC endpoint policies for S3
- S3 access points and access point policies
- KMS key policies vs grants vs aliases
- Envelope encryption patterns
- Multi-region KMS keys
- KMS key rotation behavior (automatic vs manual)
- Importing key material into KMS

### Networking Security

**[[100 - Cloud/AWS/Cloud Practitioner/Networking#VPC (Virtual Private Cloud)|Networking: VPC]]** → SAA needs:
- VPC peering limitations (no transitive peering, no overlapping CIDRs, MTU considerations)
- Transit Gateway route tables and propagation vs static routes
- VPC endpoint gateway (S3, DynamoDB) vs interface endpoint (all other services) selection criteria
- Interface endpoint security group requirements
- VPC flow logs creation, destination, and analysis patterns
- Egress-only internet gateway for IPv6
- VPC CIDR planning and secondary CIDR blocks
- VPC sharing via RAM

**[[100 - Cloud/AWS/Cloud Practitioner/Networking#Security Groups|Networking: Security Groups]]** → SAA needs:
- Security group rule ordering (all rules evaluated, no priority)
- Referencing other security groups as source/destination
- Security groups for NAT gateway (not needed, NAT GW has no SG)
- NACL rule number evaluation (lowest number wins, explicit deny at end)
- Stateful vs stateless implications for return traffic
- NACL ephemeral port ranges for outbound rules

**[[100 - Cloud/AWS/Cloud Practitioner/Networking#NAT Gateway|Networking: NAT Gateway]]** → SAA needs:
- NAT gateway high availability (one per AZ, no cross-AZ failover)
- NAT gateway cost model (hourly + per-GB processing)
- NAT instance vs NAT gateway trade-offs (bandwidth, HA, management)
- NAT gateway with Transit Gateway routing
- VPC endpoints as alternative to NAT for AWS service traffic

**[[100 - Cloud/AWS/Cloud Practitioner/Connectivity#AWS PrivateLink|Connectivity: PrivateLink]]** → SAA needs:
- Interface endpoint DNS resolution (private hosted zone, Route 53)
- Endpoint policies to restrict access
- Gateway Load Balancer endpoints for third-party virtual appliances
- VPC endpoint service creation (your own service behind NLB)
- Cross-account VPC endpoint access
- Gateway endpoints (S3, DynamoDB) route table requirements

**[[100 - Cloud/AWS/Cloud Practitioner/Connectivity|Connectivity]]** → SAA needs:
- Site-to-Site VPN tunnel redundancy and BGP configuration
- Client VPN authentication methods (mutual cert, AD, federated)
- Direct Connect transit VIF vs private VIF
- Direct Connect gateway for multi-Region access
- VPN CloudHub for site-to-site mesh
- Direct Connect + VPN as active/passive or active/active
- MACsec encryption on Direct Connect

**[[100 - Cloud/AWS/Cloud Practitioner/Connectivity#AWS Transit Gateway|Connectivity: Transit Gateway]]** → SAA needs:
- Transit Gateway route tables (isolation, segmentation)
- Attachment types (VPC, VPN, Direct Connect, peering)
- Transit Gateway peering (cross-Region)
- Transit Gateway shareability via RAM
- Transit Gateway multicast
- Transit Gateway Connect attachment (GRE tunnels for third-party appliances)
- Route propagation vs static routes

### Application Security

**[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS WAF (Web Application Firewall)|Security: WAF]]** → SAA needs:
- WAF rule types (managed rule groups, rate-based rules, regex pattern sets, geo match, IP sets, bot control)
- WAF web ACL association (CloudFront, ALB, AppSync, API Gateway)
- WAF logging to S3/Kinesis/Firehose
- WAF pricing (per web ACL, per rule, per request)
- WAF vs Shield vs Network Firewall positioning

**[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Shield|Security: Shield]]** → SAA needs:
- Shield Standard (automatic, free) vs Shield Advanced (DDoS cost protection, 24/7 DRT, WAF integration)
- Shield Advanced protected resource types (CloudFront, Route 53, Global Accelerator, ALB, EIP)
- Shield response team (DRT) engagement
- Shield Advanced health checks and proactive engagement

**[[100 - Cloud/AWS/Cloud Practitioner/Security#Amazon GuardDuty|Security: GuardDuty]]** → SAA needs:
- GuardDuty data sources (VPC flow logs, CloudTrail, DNS logs, EKS audit logs, S3 data events, Lambda logs)
- GuardDuty finding types (recon, instance compromise, credential access, exfiltration)
- Trusted IP lists and threat lists
- GuardDuty multi-account setup via Organizations
- GuardDuty cannot be disabled per-account once enabled at org level

**[[100 - Cloud/AWS/Cloud Practitioner/Security#Amazon Macie|Security: Macie]]** → SAA needs:
- Macie automated sensitive data discovery jobs
- Custom data identifiers (regex + keyword)
- Macie findings types (policy, sensitive data)
- Macie coverage for S3 buckets (auto-enable for new buckets via Organizations)
- Macie integration with Security Hub

**[[100 - Cloud/AWS/Cloud Practitioner/Security#Amazon Inspector|Security: Inspector]]** → SAA needs:
- Inspector scanning targets (EC2, ECR, Lambda)
- Inspector rule packages (network reachability, OS vulnerabilities, package vulnerabilities)
- Inspector findings and remediation
- Inspector vs GuardDuty vs Security Hub positioning

**[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Security Hub|Security: Security Hub]]** → SAA needs:
- Security Hub standards (CIS, AWS Foundational Security Best Practices, PCI DSS)
- Cross-Region and cross-account aggregation
- Finding workflows (suppression, resolution)
- Security Hub as aggregator for GuardDuty/Inspector/Macie/Config findings
- Custom actions and EventBridge integration

**[[100 - Cloud/AWS/Cloud Practitioner/Security#Amazon Detective|Security: Detective]]** → SAA needs:
- Detective graph behavior analysis (VPC flow logs, CloudTrail, GuardDuty findings)
- Detective use cases (investigate GuardDuty findings, identify root cause)
- Detective vs Security Hub vs GuardDuty positioning
- Automatic data ingestion and retention

### Data Security & Encryption

**[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS KMS (Key Management Service)|Security: KMS]]** → SAA needs:
- Symmetric vs asymmetric KMS keys
- KMS key policies (who can use vs manage)
- KMS grants (programmatic access delegation)
- KMS aliases
- Automatic vs manual key rotation
- Multi-region KMS keys
- KMS key deletion waiting period (7-30 days)
- Envelope encryption with data keys
- KMS integration with all encryptable services

**[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS CloudHSM|Security: CloudHSM]]** → SAA needs:
- CloudHSM use cases (FIPS 140-2 Level 3, custom PKCS#11/JCA/JCE access, cloud HSM cluster)
- CloudHSM vs KMS trade-offs (single-tenant vs multi-tenant, compliance requirements)
- CloudHSM high availability (multi-AZ cluster)
- CloudHSM key backup and restore

**[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Certificate Manager (ACM)|Security: ACM]]** → SAA needs:
- ACM public vs private certificates
- DNS validation vs email validation
- ACM certificate association (ELB, CloudFront, API Gateway)
- ACM cannot export private key
- ACM certificate renewal behavior
- Private CA integration with ACM

**[[100 - Cloud/AWS/Cloud Practitioner/Security#Data Protection|Security: Data Protection]]** / **[[100 - Cloud/AWS/Cloud Practitioner/S3#Security|S3: Security]]** / **[[100 - Cloud/AWS/Cloud Practitioner/Database|Database]]** → SAA needs:
- Service-specific encryption implementation (EBS encryption at rest, RDS encryption at rest, DynamoDB encryption at rest, S3 SSE-S3 vs SSE-KMS vs SSE-C)
- Customer-managed vs AWS-managed vs customer-provided keys
- Encryption cannot be enabled on existing unencrypted EBS/RDS (must create new)
- Cross-account encrypted resource sharing

**[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Secrets Manager|Security: Secrets Manager]]** → SAA needs:
- Secrets Manager automatic rotation (Lambda function, rotation schedule)
- Secrets Manager vs SSM Parameter Store trade-offs (cost, rotation, cross-account)
- Secrets Manager integration with RDS
- Cross-account secret sharing via resource policies
- Secret versioning and staging labels

**[[100 - Cloud/AWS/Cloud Practitioner/S3#Object Lifecycle Management|S3: Lifecycle]]** → SAA needs:
- S3 lifecycle rules (transition, expiration, abort incomplete multipart uploads)
- S3 Object Lock modes (governance vs compliance) and retention periods
- S3 lifecycle with Object Lock interaction
- Glacier Vault Lock for immutable archives
- S3 replication (CRR, SRR) and replication time control

### Compliance & Governance

**[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Shared Responsibility Model|Security: Shared Responsibility]]** → SAA needs:
- Service-specific responsibility boundaries (EC2 = customer manages OS/patching, RDS = AWS manages OS/patching, Lambda = AWS manages everything except code, ECS/Fargate differences)
- Shared controls (patch management, configuration management, awareness and training)

**[[100 - Cloud/AWS/Cloud Practitioner/Governance#AWS Artifact|Governance: Artifact]]** → SAA needs:
- Artifact reports (SOC, PCI, ISO)
- Artifact agreements (BAA, NDA)
- Using Artifact reports for compliance audits
- Artifact report download and sharing

**[[100 - Cloud/AWS/Cloud Practitioner/Governance#AWS Audit Manager|Governance: Audit Manager]]** → SAA needs:
- Audit Manager frameworks (pre-built and custom)
- Evidence collection automation
- Audit Manager integration with Config/CloudTrail/Security Hub
- Audit reports generation
- Cross-account and cross-Region evidence collection

**[[100 - Cloud/AWS/Cloud Practitioner/Governance#AWS Config|Governance: Config]]** → SAA needs:
- Config rules (managed vs custom, Lambda-backed)
- Config remediation actions (SSM Automation documents)
- Config conformance packs
- Config multi-account multi-Region aggregation
- Config timeline for resource changes
- Config vs CloudTrail vs Security Hub positioning

---

## Domain 2: Design Resilient Architectures (26%)

### Scalable & Loosely Coupled Architectures

**[[100 - Cloud/AWS/Cloud Practitioner/Messaging#Amazon SQS (Simple Queue Service)|Messaging: SQS]]** → SAA needs:
- Visibility timeout tuning strategies
- Dead-letter queue configuration and redrive policies
- Long polling vs short polling
- Message deduplication for FIFO (content-based vs message group ID)
- Ordering guarantees and limitations
- Maximum batch sizes
- Delay queues
- SQS as a decoupling pattern in multi-tier apps
- Exactly-once processing trade-offs
- Message retention period selection
- Cost comparison standard vs FIFO

**[[100 - Cloud/AWS/Cloud Practitioner/Messaging#Amazon SNS (Simple Notification Service)|Messaging: SNS]]** → SAA needs:
- Fan-out architecture to SQS/Lambda/HTTP
- Message filtering with subscription filter policies
- Cross-account SNS topics
- FIFO topics and ordering
- SNS vs EventBridge decision criteria
- Message deduplication
- Delivery status logging
- Payload size limits and large message patterns (SNS + S3)
- Raw message delivery for Lambda/SQS

**[[100 - Cloud/AWS/Cloud Practitioner/Messaging#Amazon EventBridge|Messaging: EventBridge]]** → SAA needs:
- Event bus architecture (default vs custom vs SaaS partner)
- Event schema registry
- Input transformation and filtering
- Archive and replay
- Cross-account event delivery
- API destinations
- EventBridge vs SNS vs SQS decision matrix
- Rule priority and ordering
- Dead-letter queue for failed invocations

**[[100 - Cloud/AWS/Cloud Practitioner/SpecializedServices#Development Services|Specialized: Dev Services]]** (AppSync) → SAA needs:
- GraphQL resolver design
- DynamoDB as AppSync data source
- Real-time subscriptions with WebSocket
- Conflict detection and resolution
- Caching at the edge
- Authorization modes (API key, IAM, Cognito, OIDC)
- Lambda resolvers
- Pipeline resolvers
- Offline sync patterns

**[[100 - Cloud/AWS/Cloud Practitioner/Networking#API Gateway|Networking: API Gateway]]** → SAA needs:
- REST vs HTTP vs WebSocket API selection
- Lambda proxy integration vs custom integration
- Request/response transformation (mapping templates)
- Usage plans and API keys
- Mutual TLS (mTLS)
- VPC link for private integrations
- Stage variables
- Canary deployments
- Throttling at account vs stage vs method level
- Caching TTL configuration
- Authorizers (Lambda, Cognito, IAM)
- OpenAPI/Swagger import

**[[100 - Cloud/AWS/Cloud Practitioner/Messaging|Messaging]]** (Event-driven architecture patterns) → SAA needs:
- Choreography vs orchestration patterns
- Saga pattern for distributed transactions
- Event sourcing
- CQRS
- Poison pill message handling
- Idempotency design
- Event schema evolution
- Ordering across partitions
- Exactly-once vs at-least-once trade-offs
- Circuit breaker pattern

**[[100 - Cloud/AWS/Cloud Practitioner/Compute#Elasticity (Scaling)|Compute: Scaling]]** → SAA needs:
- Target tracking vs step scaling vs simple scaling policies
- Scale-out vs scale-in cooldown periods
- Predictive scaling with ML
- Custom CloudWatch metrics for scaling
- Lifecycle hooks for warm-up/cool-down
- Scaling with SQS queue depth (approximate number of messages visible)
- ECS service auto-scaling
- DynamoDB auto-scaling
- Aurora auto-scaling read replicas

**[[100 - Cloud/AWS/Cloud Practitioner/Networking#CDN: CloudFront|Networking: CloudFront]]** (Caching strategies) → SAA needs:
- Cache invalidation strategies (versioned URLs, invalidation API, Cache-Control headers)
- Origin shield architecture
- Signed URLs vs signed cookies
- Lambda@Edge vs CloudFront Functions decision
- Cache behavior priority
- Geo-restriction
- Field-level encryption
- Response headers policy
- Origin failover
- S3 as origin vs custom origin
- WebSocket support via CloudFront

### High Availability & Fault Tolerance

**[[100 - Cloud/AWS/Cloud Practitioner/Networking#DNS: Route 53|Networking: Route 53]]** → SAA needs:
- [x] Routing policy selection scenarios (when to use weighted vs latency vs failover vs geoproximity)
- [x] Health check types (HTTP/HTTPS/TCP, endpoint vs CloudWatch alarm-based)
- [x] Calculated health checks
- [x] Private hosted zones
- [x] Routing to AWS resources (alias records vs CNAME)
- [x] Failover with active-passive vs active-active
- [x] Latency-based routing with weighted records
- [x] Multivalue answer routing vs simple routing with multiple values

**[[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon RDS|Database: RDS]]** / **[[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon ElastiCache|Database: ElastiCache]]** (Multi-AZ deployments) → SAA needs:
- Multi-AZ vs read replicas (when to use each)
- Automatic failover mechanics and DNS switchover time
- Multi-AZ for RDS engine support (all vs Aurora-specific)
- ElastiCache Multi-AZ with automatic failover (Redis only, not Memcached)
- EFS availability across AZs
- Multi-AZ deployment cost implications
- RDS Multi-AZ cluster (two readable standbys vs single standby)

**[[100 - Cloud/AWS/Cloud Practitioner/SpecializedServices#Development Services|Specialized: Dev Services]]** (X-Ray) → SAA needs:
- Service map interpretation
- Sampling rules and rates
- Instrumentation with X-Ray SDK
- Annotations vs metadata
- Trace header propagation across services
- X-Ray with Lambda, API Gateway, ECS, EC2
- Integrating X-Ray with CloudWatch Logs Insights
- Group and filter expressions
- Fault vs error vs throttle identification

---

## Domain 3: Design High-Performing Architectures (24%)

### Storage Performance

**[[100 - Cloud/AWS/Cloud Practitioner/S3|S3]]** → SAA needs:
- [x] S3 performance optimization via request rate partitioning (prefix randomization no longer needed but still tested)
- [x] Multipart upload tuning
- S3 Select for filtering
- [x] Byte-range fetches
- S3 Object Lambda
- S3 Multi-Region Access Points
- [x] S3 Transfer Acceleration vs CloudFront trade-off decision scenarios
- [x] S3 performance with CloudFront origin shield
- [x] S3 lifecycle transition timing and cost implications
- [x] S3 event notification fanout patterns with SQS/SNS/Lambda

**[[100 - Cloud/AWS/Cloud Practitioner/FileStorage#Amazon FSx|FileStorage: FSx]]** → SAA needs:
- FSx Lustre S3-backed data repository associations (HSM vs SSD throughput modes)
- FSx Windows multi-AZ deployment patterns
- FSx ONTAP storage virtual machines and FlexVol sizing
- FSx OpenZFS use cases vs Lustre
- Throughput mode selection (bursting vs provisioned) per workload
- FSx backup and restore strategies

**[[100 - Cloud/AWS/Cloud Practitioner/StorageGateway|StorageGateway]]** → SAA needs:
- Storage Gateway volume gateway cached vs stored mode selection scenarios
- Tape gateway VTL migration patterns
- File gateway NFS vs SMB performance tuning
- Storage Gateway with S3 lifecycle policies
- Hybrid architecture with Direct Connect vs Site-to-Site VPN for gateway traffic
- Gateway VM sizing and caching disk requirements

**[[100 - Cloud/AWS/Cloud Practitioner/Storage|Storage]]** → SAA needs:
- EBS volume type selection scenarios (gp3 vs io2 vs st1 vs sc1) with IOPS/throughput calculations
- EBS multi-attach patterns with cluster filesystems
- EBS snapshot lifecycle and cross-region copy
- Instance store vs EBS trade-offs for specific workloads
- EFS performance modes (general purpose vs max I/O) and throughput modes (bursting vs provisioned vs elastic)
- EFS lifecycle policies and Infrequent Access tier
- FSx vs EFS decision matrices

### Compute Performance

**[[100 - Cloud/AWS/Cloud Practitioner/Compute#AWS Lambda|Compute: Lambda]]** → SAA needs:
- Lambda memory/CPU proportional allocation tuning
- Provisioned Concurrency vs on-demand trade-offs and cost analysis
- Lambda destinations and retry policies
- Lambda with EFS mount for cold start mitigation
- Lambda SnapStart for Java workloads
- Lambda ephemeral storage sizing
- Lambda concurrency limits (account-level vs reserved vs provisioned)
- Lambda in VPC (ENI scaling, NAT gateway considerations)
- Lambda event source mapping batch window/item handling
- Lambda Powertools and layer sharing patterns

**[[100 - Cloud/AWS/Cloud Practitioner/Containers#AWS Fargate|Containers: Fargate]]** → SAA needs:
- Fargate task sizing (vCPU/memory combinations)
- Fargate ephemeral storage limits
- Fargate platform versions and differences
- Fargate with EFS for persistent storage
- Fargate networking modes (awsvpc vs bridge)
- Fargate cost optimization vs EC2 launch type
- Fargate with Application Auto Scaling and target tracking
- Fargate task IAM roles vs task execution roles

**[[100 - Cloud/AWS/Cloud Practitioner/Containers|Containers]]** → SAA needs:
- ECS service types (replica vs daemon)
- ECS scheduling strategies (binpack vs spread vs random)
- ECS capacity providers (Fargate vs EC2 vs external)
- ECS service discovery via Cloud Map
- ECS task placement constraints and strategies
- EKS node group types (managed vs self-managed vs Fargate profiles)
- EKS networking (CNI plugin, security groups for pods)
- EKS cluster autoscaler vs Karpenter
- ECS vs EKS decision criteria for exam scenarios

**[[100 - Cloud/AWS/Cloud Practitioner/Compute#AWS Batch|Compute: Batch]]** → SAA needs:
- AWS Batch compute environments (managed vs unmanaged)
- Job queue priority and ordering
- Job definition retry strategies and timeout
- Batch with Spot Instances and fallback
- Batch array jobs vs multi-node parallel jobs
- Batch job dependencies and DAGs
- Batch with EFS/EFS for shared data

**[[100 - Cloud/AWS/Cloud Practitioner/Analytics#Amazon EMR|Analytics: EMR]]** → SAA needs:
- EMR cluster types (transient vs long-running)
- EMR instance fleet vs instance group
- EMR with Spot Instances and instance fleet allocation strategies
- EMR step vs bootstrap action
- EMRFS consistent view with S3
- EMR auto-scaling policies
- EMR on EKS architecture
- EMR Serverless vs cluster mode

**[[100 - Cloud/AWS/Cloud Practitioner/Compute#AWS Outposts|Compute: Outposts]]** / **[[100 - Cloud/AWS/Cloud Practitioner/Global Infrastructure#Outposts|Global Infra: Outposts]]** → SAA needs:
- Outposts rack vs server form factors
- Outposts local gateway vs local zone routing
- Outposts S3 on Outposts for ultra-low latency
- Outposts RDS on Outposts
- Outposts networking (Direct Connect to Outposts, subnet extension)
- Outposts failure modes and AWS responsibility boundary
- Outposts vs Wavelength vs Local Zone decision scenarios

### Database Performance

**[[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon RDS (Relational Database Service)|Database: RDS]]** → SAA needs:
- RDS engine-specific Multi-AZ implementations (SQL Server mirroring vs PostgreSQL synchronous replication vs MySQL semi-sync)
- RDS read replica lag troubleshooting
- RDS Proxy connection pooling scenarios and cost
- RDS Performance Insights for query tuning
- RDS parameter group tuning
- RDS cross-region read replica promotion for DR
- RDS Custom for OS-level access
- RDS storage autoscaling
- RDS backup retention and PITR mechanics
- RDS engine version upgrade strategies (blue/green deployments)

**[[100 - Cloud/AWS/Solutions Architect Associate/RDS#Amazon Aurora|RDS: Aurora]]** → SAA needs:
- ~~Aurora storage architecture (6 copies across 3 AZs, self-healing)~~
- ~~Aurora read replica lag vs RDS replicas (sub-ms)~~
- Aurora Serverless v1 vs v2 differences and use cases
- Aurora Global Database cross-region replication lag (<1s)
- Aurora fast cloning for dev/test
- Aurora parallel query for analytics offload
- Aurora multi-master (write forwarding)
- Aurora RDS Proxy integration
- ~~Aurora backtracking (point-in-time without restore)~~
- Aurora machine learning integration
- Aurora I/O-Optimized storage class

**[[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon DynamoDB|Database: DynamoDB]]** → SAA needs:
- Partition key design to avoid hot partitions (high-cardinality, composite keys)
- GSI vs LSI trade-offs (LSI shares partition key, limited to 5 per table, inherited throughput; GSI has own throughput, unlimited)
- GSI projection types (ALL vs KEYS_ONLY vs INCLUDE) and cost impact
- DynamoDB Streams use cases (Lambda triggers, cross-region replication, Kinesis Data Streams integration)
- DAX caching patterns (write-through, cache invalidation)
- DynamoDB Transactions (read/write, idempotency)
- DynamoDB Adaptive Capacity for imbalanced workloads
- DynamoDB TTL for automatic expiration
- DynamoDB on-demand vs provisioned capacity with auto-scaling
- DynamoDB global tables (multi-region active-active, conflict resolution last-writer-wins)
- DynamoDB Accelerator (DAX) vs ElastiCache decision
- DynamoDB PartiQL for SQL-like queries
- DynamoDB point-in-time recovery

**[[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon ElastiCache|Database: ElastiCache]]** → SAA needs:
- [x] Redis vs Memcached decision criteria (persistence, replication, multi-threading, data structures)
- [x] Redis cluster mode enabled vs disabled
- [x] Redis replication groups and multi-AZ with auto-failover
- [x] Redis backup and restore
- [x] Redis AUTH and encryption in-transit/at-rest
- ElastiCache Serverless vs provisioned
- ElastiCache Global Datastore for cross-region DR
- [x] ElastiCache with DynamoDB DAX comparison
- Cache stampede/thundering herd mitigation patterns
- [x] Cache-aside vs write-through vs write-behind strategies

**[[100 - Cloud/AWS/Cloud Practitioner/Analytics#Amazon Redshift|Analytics: Redshift]]** → SAA needs:
- Redshift node types (RA3 with managed storage vs DC2)
- Redshift distribution styles (KEY vs EVEN vs ALL) and sort keys
- Redshift sort key types (compound vs interleaved)
- Redshift concurrency scaling for burst workloads
- Redshift Spectrum for S3 querying without loading
- Redshift data sharing across clusters/accounts
- Redshift Serverless vs provisioned
- Redshift materialized views vs regular views
- Redshift workload management (WLM) queues
- Redshift cross-region snapshot copy for DR
- Redshift VACUUM and ANALYZE maintenance

**[[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon Neptune|Database: Neptune]]** → SAA needs:
- Neptune use cases (knowledge graphs, fraud detection, recommendation engines)
- Neptune Gremlin vs SPARQL query languages
- Neptune read replicas and Multi-AZ
- Neptune global database for cross-region
- Neptune fast clone
- Neptune with IAM authentication

**[[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon DocumentDB|Database: DocumentDB]]** → SAA needs:
- DocumentDB use cases (content management, catalogs, user profiles)
- DocumentDB vs MongoDB compatibility gaps
- DocumentDB global clusters
- DocumentDB read replicas and auto-scaling
- DocumentDB backup and restore
- DocumentDB vs DynamoDB decision scenarios

**[[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#Migration Services|CAF: Migration]]** (DMS) → SAA needs:
- DMS migration types (full load, CDC, full load + CDC)
- DMS endpoint types (source/target)
- DMS task settings (batch size, parallel threads, LOB handling)
- DMS schema conversion tool (SCT) for heterogeneous migrations
- DMS continuous replication for zero-downtime cutover
- DMS with S3 as target for data lake
- DMS limitations (DDL changes, sequence handling)
- SCT assessment reports and conversion complexity scoring

**[[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon RDS|Database: RDS]]** (read replicas and caching) → SAA needs:
- [x] Read replica vs Multi-AZ distinction (read scaling vs HA)
- [x] Read replica promotion to standalone
- [x] Read replica lag monitoring and mitigation
- [x] ElastiCache cache invalidation strategies (TTL, write-through, cache-aside)
- [x] ElastiCache cluster vs replication group architecture
- [x] Read replica for cross-region DR strategy
- RDS Proxy for connection storm mitigation

### Network Performance

**[[100 - Cloud/AWS/Cloud Practitioner/Networking#CDN: CloudFront|Networking: CloudFront]]** → SAA needs:
- CloudFront cache behaviors and path patterns
- CloudFront origin groups for failover
- CloudFront signed URLs vs signed cookies
- CloudFront with AWS WAF integration
- CloudFront origin access control (OAC) vs origin access identity (OAI) for S3
- CloudFront Lambda@Edge vs CloudFront Functions (runtime limits, use cases)
- CloudFront real-time logs
- CloudFront custom headers to origin
- CloudFront with ALB origin
- CloudFront Geo Restriction
- CloudFront field-level encryption
- CloudFront compression settings
- CloudFront invalidation vs versioned objects

**[[100 - Cloud/AWS/Cloud Practitioner/Networking#Global Accelerator|Networking: Global Accelerator]]** → SAA needs:
- Global Accelerator vs CloudFront decision matrix (HTTP/S caching vs any TCP/UDP, edge caching vs direct to origin)
- Global Accelerator endpoint groups and traffic dials
- Global Accelerator health checks and failover
- Global Accelerator with NLB/ALB/EC2/Elastic IP endpoints
- Global Accelerator client IP preservation
- Global Accelerator flow logs
- Global Accelerator custom routing

**[[100 - Cloud/AWS/Cloud Practitioner/Networking#Global Accelerator|Networking: Global Accel]]** (CloudFront vs Global Accelerator) → SAA needs:
- Scenario-based decision framework: use CloudFront for static/dynamic content caching at edge, use Global Accelerator for non-cacheable TCP/UDP traffic, latency-sensitive APIs, or when you need static anycast IPs
- Exam scenarios often present both as options; key discriminator is "caching needed?" -> CloudFront, "static IP + any TCP?" -> GA

**[[100 - Cloud/AWS/Cloud Practitioner/Networking#VPC (Virtual Private Cloud)|Networking: VPC]]** → SAA needs:
- VPC endpoint types (Gateway: S3/DynamoDB vs Interface: PrivateLink for everything else)
- VPC endpoint policies and routing
- VPC peering limitations (no transitive peering, overlapping CIDR blocks)
- Transit Gateway route tables and propagation
- Transit Gateway attachments (VPC, VPN, Direct Connect, Connect)
- Transit Gateway multicast
- VPC flow logs for troubleshooting
- VPC CIDR planning for multi-account
- NAT gateway vs NAT instance vs VPC endpoint for egress
- IPv6 in VPC
- VPC sharing via RAM

**[[100 - Cloud/AWS/Cloud Practitioner/Connectivity#AWS PrivateLink|Connectivity: PrivateLink]]** → SAA needs:
- PrivateLink vs VPC peering vs Transit Gateway decision scenarios
- PrivateLink endpoint service configuration (acceptance required vs auto-accept)
- PrivateLink with NLB as service provider
- PrivateLink cross-account access
- PrivateLink DNS resolution (regional vs zonal endpoints)
- PrivateLink vs Gateway endpoints (S3/DynamoDB don't use PrivateLink)
- PrivateLink pricing (per AZ + data processing)

**[[100 - Cloud/AWS/Cloud Practitioner/Connectivity#AWS Direct Connect|Connectivity: Direct Connect]]** → SAA needs:
- Direct Connect transit VIF vs private VIF vs public VIF
- Direct Connect gateway for multi-region/multi-VPC
- Direct Connect + VPN as high-availability MACsec over Direct Connect
- Direct Connect link aggregation groups (LAG)
- Direct Connect hosting types (partner vs AWS)
- Direct Connect routing (BGP) and route filtering
- Direct Connect cost model vs Site-to-Site VPN
- Direct Connect with Transit Gateway

### Data Ingestion & Transformation

**[[100 - Cloud/AWS/Cloud Practitioner/Analytics#Data Ingestion|Analytics: Ingestion]]** → SAA needs:
- Kinesis Data Streams shard sizing and resharding
- Kinesis Data Streams retention period (1-365 days)
- Kinesis enhanced fan-out vs shared throughput
- Kinesis Data Firehose transformations (Lambda, dynamic partitioning)
- Kinesis Data Firehose buffering intervals and size
- Kinesis vs MSK decision criteria
- Kinesis Video Streams use cases
- Kinesis Data Analytics for real-time SQL processing

**[[100 - Cloud/AWS/Cloud Practitioner/Analytics#AWS Glue|Analytics: Glue]]** / **[[100 - Cloud/AWS/Cloud Practitioner/Analytics#AWS Glue Data Catalog|Analytics: Glue Catalog]]** → SAA needs:
- Glue job types (Spark, Python Shell, Ray)
- Glue Dynamic Frame vs Spark DataFrame
- Glue bookmarks for incremental ETL
- Glue Data Catalog as central metadata store (Athena, Redshift Spectrum, EMR all use it)
- Glue crawlers (schedule, custom classifiers)
- Glue workflow and triggers for orchestration
- Glue Flex instances for cost optimization
- Glue streaming ETL
- Glue Data Quality rules

**[[100 - Cloud/AWS/Cloud Practitioner/Analytics#Amazon Athena|Analytics: Athena]]** → SAA needs:
- Athena partitioning and bucketing for query performance
- Athena CTAS (CREATE TABLE AS SELECT) and INSERT INTO patterns
- Athena federated query (Lambda data sources)
- Athena workgroups for cost control
- Athena query result encryption
- Athena vs Redshift Spectrum decision
- Athena Iceberg table support

---

## Domain 4: Design Cost-Optimized Architectures (20%)

### Storage Cost Optimization

**[[100 - Cloud/AWS/Cloud Practitioner/S3#S3 Storage Classes|S3: Storage Classes]]** / **[[100 - Cloud/AWS/Cloud Practitioner/S3#Object Lifecycle Management|S3: Lifecycle]]** → SAA needs:
- [x] Minimum storage duration charges (30d IA, 90d Glacier, 180d Deep Archive)
- [x] Early deletion fee calculations
- [x] Retrieval cost trade-offs per class (expedited vs standard vs bulk)
- [x] One Zone-IA vs Standard IA decision scenarios
- [x] Lifecycle policy design for multi-stage cost optimization
- S3 Object Lock cost implications
- [x] Cross-region replication cost impact

**[[100 - Cloud/AWS/Cloud Practitioner/S3#S3 Storage Classes|S3: Storage Classes]]** (Intelligent-Tiering) → SAA needs:
- [x] Monitoring/automation fee per object
- [x] When Intelligent-Tiering is cheaper than manual lifecycle policies
- [x] Archive access tier costs
- Custom tier configuration (e.g., 7d/14d/30d archives)
- Minimum object size for cost-effectiveness
- Non-retrievable archive tier trade-offs

**[[100 - Cloud/AWS/Cloud Practitioner/BlockStorage#EBS Snapshots|BlockStorage: Snapshots]]** → SAA needs:
- [x] Incremental snapshot cost model (only changed blocks)
- [x] Cross-region snapshot copy costs
- [x] DLM (Data Lifecycle Manager) policy design for cost
- [x] Fast snapshot restore pricing
- [x] Snapshot vs AMI cost considerations
- [x] EBS-recycle bin cost impact

**[[100 - Cloud/AWS/Cloud Practitioner/FileStorage#EFS Storage Classes|FileStorage: EFS Classes]]** → SAA needs:
- EFS vs EBS vs FSx cost comparison scenarios
- [x] Provisioned vs bursting vs elastic throughput cost trade-offs
- [x] EFS One Zone vs Standard cost analysis
- [x] Intelligent-Tiering for EFS
- [x] Lifecycle management policy design
- [x] Cross-AZ data transfer costs for EFS

### Compute Cost Optimization

**[[100 - Cloud/AWS/Cloud Practitioner/Compute#Auto Scaling|Compute: Auto Scaling]]** → SAA needs:
- Target tracking vs scheduled vs predictive vs step scaling for cost optimization
- Scaling cooldown cost impact
- Integrating Auto Scaling with Spot Instances
- Custom CloudWatch metric-based scaling
- Instance warm-up time cost implications
- Mixed instances policy for cost

**[[100 - Cloud/AWS/Cloud Practitioner/Compute#AWS Lambda|Compute: Lambda]]** → SAA needs:
- Memory vs duration trade-off (Lambda Power Tuning)
- Provisioned Concurrency cost vs cold start trade-off
- Ephemeral storage pricing beyond 512MB
- ARM Graviton cost savings (20%)
- When Lambda is cheaper than EC2/Fargate (break-even analysis)
- Duration billing granularity (1ms)
- S3 event vs EventBridge invocation cost patterns

**[[100 - Cloud/AWS/Cloud Practitioner/Containers#AWS Fargate|Containers: Fargate]]** → SAA needs:
- vCPU and memory pricing model vs EC2 per-second billing
- When Fargate is cheaper vs EC2 (utilization break-even)
- Spot with Fargate cost savings (up to 70%)
- Right-sizing container resource requests to avoid over-provisioning costs
- Fargate task placement cost implications

**[[100 - Cloud/AWS/Cloud Practitioner/Billing#AWS Budgets|Billing: Budgets]]** → SAA needs:
- Budget actions (auto-apply RIs, stop resources)
- Cost vs usage vs RI/Savings Plans budgets
- Forecast-based alerts
- Integrating with SNS for automated remediation
- Service-linked and tag-filtered budgets

**[[100 - Cloud/AWS/Cloud Practitioner/Billing#AWS Cost Explorer|Billing: Cost Explorer]]** → SAA needs:
- RI utilization and coverage reports
- Savings Plans utilization analysis
- Cost allocation tag filtering and grouping
- Forecasting accuracy and limitations
- Grouping by tags/accounts/services for chargeback
- Custom date range trend analysis

### Database Cost Optimization

**[[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon Aurora|Database: Aurora]]** / **[[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon DynamoDB|Database: DynamoDB]]** (Serverless) → SAA needs:
- Aurora Serverless v2 ACU scaling granularity and cost
- When serverless is cheaper than provisioned (traffic pattern analysis)
- DynamoDB on-demand vs provisioned cost break-even point
- Aurora Serverless pause/resume cost behavior
- DynamoDB reserved capacity for predictable workloads

### Network Cost Optimization

**[[100 - Cloud/AWS/Cloud Practitioner/Connectivity#AWS PrivateLink|Connectivity: PrivateLink]]** (VPC Endpoints) → SAA needs:
- Gateway endpoint (free) vs Interface endpoint (hourly + data processing) cost comparison
- When endpoints save money vs NAT gateway routing
- Cross-AZ interface endpoint data processing charges
- S3/DynamoDB gateway endpoint design patterns

**[[100 - Cloud/AWS/Cloud Practitioner/Connectivity|Connectivity]]** (Direct Connect vs VPN vs Internet) → SAA needs:
- Port speed pricing tiers (1Gbps vs 10Gbps)
- Dedicated vs hosted connection cost comparison
- Data transfer out pricing per tier
- VPN hourly connection cost + data transfer
- When each option is most cost-effective based on volume
- Link Aggregation Groups cost implications

---

## Management & Governance

**[[100 - Cloud/AWS/Cloud Practitioner/CloudFormation|CloudFormation]]** → SAA needs:
- Intrinsic functions (Ref, Fn::GetAtt, Fn::Join, Fn::Sub)
- Conditions, mappings, outputs
- Stack policies
- Change sets
- Stack sets for multi-account/region deployments
- Custom resources with Lambda
- Termination protection
- Update behavior (replacement vs modify)
- Rollback triggers
- Drift detection remediation
- Nested stack parameter passing
- `!ImportValue` for cross-stack references

**[[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Systems Manager|Security: Systems Manager]]** → SAA needs:
- Parameter Store tiers (Standard vs Advanced, SecureString with KMS)
- Session Manager IAM requirements and port forwarding
- Run Command vs State Manager vs Automation documents
- Patch Manager baselines and patch groups
- Maintenance Windows configuration
- Inventory metadata
- OpsCenter for operational issues
- Distributor for package deployment
- Hybrid activation management

**[[100 - Cloud/AWS/Cloud Practitioner/Monitoring#Amazon CloudWatch|Monitoring: CloudWatch]]** → SAA needs:
- Custom metrics vs standard metrics (including resolution: 1-min vs 5-min)
- CloudWatch Agent installation and configuration for OS-level metrics
- Log groups and log streams architecture
- Metric filters and alarms on logs
- Metric math expressions
- Anomaly detection alarms
- CloudWatch Logs Insights query syntax
- Composite alarms
- Cross-account cross-region dashboards
- Contributor Insights
- CloudWatch Synthetics canaries
- EventBridge vs CloudWatch Events distinction

**[[100 - Cloud/AWS/Cloud Practitioner/Monitoring#AWS CloudTrail|Monitoring: CloudTrail]]** → SAA needs:
- Management vs data events
- Organization trails
- Log file validation and integrity verification
- CloudTrail Lake for SQL-based querying
- Integration with CloudWatch Logs for real-time alerting
- S3 lifecycle policies on trail logs
- Cross-region vs multi-region trails
- Insights events for unusual API activity
- Event history retention limits

**[[100 - Cloud/AWS/Cloud Practitioner/Monitoring#AWS Trusted Advisor|Monitoring: Trusted Advisor]]** → SAA needs:
- All five pillar categories (cost, performance, security, fault tolerance, service limits)
- How to programmatically access via Support API
- Trusted Advisor vs Well-Architected Tool differences
- Which checks are free vs require Business/Enterprise support

**[[100 - Cloud/AWS/Cloud Practitioner/WellArchitected#AWS Well-Architected Tool|WellArchitected: Tool]]** → SAA needs:
- All six pillars (operational excellence, security, reliability, performance efficiency, cost optimization, sustainability)
- Lens concept (industry-specific lenses)
- Workload review process
- Risk categorization (high/medium/low)
- Improvement plan tracking
- Well-Architected Framework vs Well-Architected Tool distinction

**[[100 - Cloud/AWS/Cloud Practitioner/Governance#AWS Service Catalog|Governance: Service Catalog]]** → SAA needs:
- Portfolio creation and sharing
- Product versioning
- Constraints (launch, notification, template, stackset)
- Provisioning artifacts
- Tag options for cost allocation
- IAM integration for who can launch what
- Sharing portfolios across accounts via AWS RAM

**[[100 - Cloud/AWS/Cloud Practitioner/Governance#AWS License Manager|Governance: License Manager]]** → SAA needs:
- License configuration rules (vCPU, memory, network)
- Bringing your own license (BYOL) scenarios
- Cross-account license sharing
- Integration with EC2 and RDS
- License compliance reporting

---

## Migration & Transfer

**[[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#Migration Services|CAF: Migration]]** (MGN) → SAA needs:
- Replication server architecture
- Cutover process
- Post-launch testing
- Supported OS matrix
- Network bandwidth requirements
- Agentless vs agent-based replication
- Integration with Elastic Disaster Recovery

**[[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#Migration Services|CAF: Migration]]** (DMS) → SAA needs:
- Full load vs CDC (change data capture)
- Replication instance sizing
- Source/target endpoint configuration
- Task settings (error handling, logging)
- SCT (Schema Conversion Tool) for heterogeneous migrations
- Supported source/target combinations
- Continuous replication vs one-time migration
- Task monitoring and troubleshooting

**[[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#Migration Services|CAF: Migration]]** (DataSync) → SAA needs:
- Agent deployment (on-prem)
- Task configuration (bandwidth throttling, scheduling, verification mode)
- Source/destination types (NFS, SMB, S3, EFS, FSx, HDFS)
- Task execution monitoring
- Data integrity verification modes

**[[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#Migration Services|CAF: Migration]]** (Transfer Family) → SAA needs:
- SFTP/FTPS/FTP protocol differences
- Identity provider integration (SAML, LDAP, custom Lambda)
- VPC endpoint types (internet-facing vs VPC-only)
- Workflow feature for post-upload processing
- Security policies
- Custom identity providers vs AWS Directory Service

**[[100 - Cloud/AWS/Cloud Practitioner/CloudAdoptionFramework#7 Rs|CAF: 7 Rs]]** → SAA needs:
- Scenario-based decision trees for choosing between rehost/replatform/refactor
- Cost/benefit analysis of each strategy
- Timeline considerations
- Risk assessment per strategy
- Real-world exam scenarios requiring strategy selection

---

## ML & AI Services (awareness level)

**[[100 - Cloud/AWS/Cloud Practitioner/AI#Amazon SageMaker AI|AI: SageMaker]]** → SAA needs:
- Built-in algorithms vs custom models
- Training vs hosting endpoints
- Batch transform vs real-time inference
- SageMaker Studio vs Notebook instances
- Model monitoring and drift detection
- AutoML (SageMaker Autopilot)
- Feature store
- Pipeline orchestration
- Cost considerations for training vs inference

**[[100 - Cloud/AWS/Cloud Practitioner/AI#Amazon Rekognition|AI: Rekognition]]** → SAA needs:
- Image vs video analysis APIs
- Custom labels training
- Face comparison and collection management
- Content moderation use cases
- Integration patterns with S3 and Lambda
- Streaming video analysis

**[[100 - Cloud/AWS/Cloud Practitioner/AI#Amazon Polly|AI: Polly]]** → SAA needs:
- Standard vs neural voices
- SSML support
- Speech marks for lip-sync
- Real-time streaming vs batch synthesis
- Use case: accessibility, IVR systems, content creation

**[[100 - Cloud/AWS/Cloud Practitioner/AI#Amazon Transcribe|AI: Transcribe]]** → SAA needs:
- Real-time streaming vs batch transcription
- Custom vocabulary and language models
- Speaker identification
- PII redaction
- Medical transcription option
- Integration with S3 and Lambda

**[[100 - Cloud/AWS/Cloud Practitioner/AI#Amazon Translate|AI: Translate]]** → SAA needs:
- Real-time vs batch translation
- Custom terminology
- Auto-detect source language
- Active custom translation
- Use case: multi-language content pipelines

**[[100 - Cloud/AWS/Cloud Practitioner/AI#Amazon Comprehend|AI: Comprehend]]** → SAA needs:
- Entity recognition
- Sentiment analysis
- Key phrase extraction
- Topic modeling
- Custom entity/classifier training
- PII detection
- Real-time vs async analysis

**[[100 - Cloud/AWS/Cloud Practitioner/AI#Amazon Lex|AI: Lex]]** → SAA needs:
- Bot aliases and versions
- Intent/slot/utterance design
- Lambda fulfillment integration
- Session attributes
- Multi-turn conversations
- Integration with Connect and messaging platforms
- V2 vs V1 API differences

**[[100 - Cloud/AWS/Cloud Practitioner/AI#Amazon Kendra|AI: Kendra]]** → SAA needs:
- Index configuration
- Data source connectors
- FAQ vs document ingestion
- Query understanding
- Experience boosting
- User context filtering
- S3/SharePoint/Confluence/RDS connectors

**[[100 - Cloud/AWS/Cloud Practitioner/AI#Amazon Textract|AI: Textract]]** → SAA needs:
- Document analysis (forms, tables) vs detect text only
- Asynchronous vs synchronous APIs
- Integration with S3 and Lambda
- Use case: invoice processing, KYC workflows, expense reports

---

## Other In-Scope Services

**[[100 - Cloud/AWS/Cloud Practitioner/SpecializedServices#Development Services|Specialized: Dev Services]]** (Amplify) → SAA needs:
- CI/CD pipeline for front-end
- Custom domains and branch management
- Hosting vs full-stack (with backend)
- Environment variables
- Pull previews
- Integration with Cognito and AppSync

**[[100 - Cloud/AWS/Cloud Practitioner/Compute#Elastic Beanstalk|Compute: Beanstalk]]** → SAA needs:
- [x] Deployment policies (all-at-once, rolling, immutable, blue/green)
- [x] Environment tiers (web server vs worker)
- [x] Configuration files (.ebextensions)
- [x] Platform customization
- [x] Managed platform updates
- [x] Integration with RDS and ElastiCache
- [x] Scaling configuration
- [x] Custom domain setup

**[[100 - Cloud/AWS/Cloud Practitioner/Analytics#Amazon QuickSight|Analytics: QuickSight]]** → SAA needs:
- SPICE vs direct query
- Dashboard vs analysis vs dataset hierarchy
- Row-level security
- Embedding dashboards
- Scheduled email reports
- Data source connectors
- Cost model (Standard vs Enterprise edition)

---

## Critical Unchecked Items (Not Yet Studied at Any Level)

These items are not covered by any existing note and need dedicated study:

### Security
- IAM policy evaluation logic (allow/deny, SCPs, resource policies)
- Cross-account access (STS, AssumeRole, resource-based policies)
- Federation (SAML 2.0, OIDC, Cognito identity pools)
- MFA enforcement, root user best practices
- AWS Directory Service federation with IAM roles
- AWS Network Firewall
- Network segmentation strategies
- AWS Firewall Manager (centralized WAF/Shield management)
- S3 Object Lock, Glacier Vault Lock
- Amazon Cognito (user pools vs identity pools)

### Resilience
- Disaster recovery strategies (backup/restore, pilot light, warm standby, active-active)
- RPO and RTO concepts
- Route 53 health checks
- Multi-Region architectures
- Failover strategies
- Immutable infrastructure patterns
- Service quotas and throttling
- RDS Proxy (connection pooling)
- Microservices design principles (stateless vs stateful)
- Multi-tier architecture design
- Amazon MQ (managed message broker)
- AWS Step Functions (state machines, workflow orchestration)

### Performance
- EC2 instance families and selection (compute/memory/storage/GPU optimized)
- [x] S3 Transfer Acceleration vs CloudFront
- AWS Wavelength (edge compute for 5G)
- Amazon Keyspaces (Cassandra-compatible)
- Amazon MSK (Managed Streaming for Kafka)
- AWS Lake Formation (data lake governance)

### Cost
- EC2 pricing models (On-Demand, Reserved, Spot, Savings Plans, Dedicated)
- Spot Instances (Spot Fleet, interruption handling)
- Reserved Instances (Standard vs Convertible, regional vs zonal)
- Savings Plans (Compute vs EC2 Instance vs SageMaker)
- Instance right-sizing (Compute Optimizer)
- EC2 hibernation for cost savings
- AWS Cost and Usage Report
- Cost allocation tags
- NAT gateway costs (per-AZ vs shared, data processing charges)
- Transit Gateway costs (attachment hours, data processing)
- CloudFront pricing and caching to reduce origin costs
- Data transfer cost minimization (same-AZ, private IPs)
- Bandwidth allocation planning
- RDS pricing (On-Demand vs Reserved, Aurora pricing)
- DynamoDB capacity modes (on-demand vs provisioned, reserved capacity)
- DynamoDB DAX cost trade-offs
- ElastiCache node selection and reserved nodes
- Redshift pricing (on-demand vs reserved, Spectrum, concurrency scaling)
- Database backup retention cost impact
- EBS volume type selection and sizing
- FSx cost considerations
- Backup strategies and cost trade-offs
- Data transfer costs (ingress/egress, cross-region, internet)
- [x] S3 Requester Pays

### Management & Governance
- AWS Resource Access Manager (RAM)
- AWS CLI essentials

### Migration & Transfer
- AWS Snow Family (Snowcone, Snowball, Snowmobile)

### Other
- AWS Serverless Application Repository
- Amazon AppFlow (SaaS data integration)
- AWS Device Farm (mobile testing)
- Amazon Elastic Transcoder (media transcoding)
- Amazon Kinesis Video Streams
- Amazon Managed Grafana
- Amazon Managed Service for Prometheus
- AWS Compute Optimizer
- AWS Data Exchange

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
