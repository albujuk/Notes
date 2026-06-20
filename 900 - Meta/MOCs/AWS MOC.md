---
type: moc
tags:
  - moc
  - aws
---

# AWS MOC

How the AWS notes connect. The folder index ([[100 - Cloud/AWS/Cloud Practitioner/README|Cloud Practitioner README]]) lists files in learning order; this MOC groups them by concept.

## Compute spectrum

From most user-managed to least:

- [[Compute]]: EC2 (VMs you control), Lambda (serverless functions), Beanstalk, Batch, Lightsail, Outposts
- [[100 - Cloud/AWS/Solutions Architect Associate/EC2|EC2 (SAA)]]: placement groups, ENI, hibernation, Auto Scaling Groups, instance bootstrapping
- [[100 - Cloud/AWS/Solutions Architect Associate/Elastic Beanstalk|Elastic Beanstalk (SAA)]]: managed app platform, components, tiers, Golden AMI + User Data, deployment policies
- [[100 - Cloud/AWS/Solutions Architect Associate/Lambda|Lambda (SAA)]]: concurrency (reserved vs provisioned), cold starts, execution environments
- [[Containers]]: ECR (registry), ECS (AWS-native), EKS (Kubernetes), Fargate (serverless containers)

Trade-off: more control ↔ less ops overhead.

## Storage tiers

- [[Storage]]: block vs object vs file overview
- [[BlockStorage]]: EC2 instance store, EBS, snapshots
- [[100 - Cloud/AWS/Cloud Practitioner/S3]]: object storage, classes, lifecycle
- [[100 - Cloud/AWS/Solutions Architect Associate/S3|S3 (SAA)]]: buckets, versioning, replication, storage classes (deep dive), lifecycle rules, Express One Zone, event notifications, performance, batch operations
- [[FileStorage]]: EFS, FSx
- [[100 - Cloud/AWS/Solutions Architect Associate/EFS|EFS (SAA)]]: NFS file system, performance/throughput modes, storage classes
- [[100 - Cloud/AWS/Solutions Architect Associate/Storage Extras|Storage Extras (SAA)]]: Snow Family, FSx (all types), Storage Gateway, Transfer Family, DataSync, hybrid cloud
- [[StorageGateway]]: hybrid cloud bridge

Pick by access pattern: random IO → block; HTTP/API → object; POSIX shared → file.

## Networking + connectivity

- [[100 - Cloud/AWS/Cloud Practitioner/Networking|Networking]]: VPC, subnets, route tables, IGW/NAT, security groups vs NACLs, Route 53, CloudFront
- [[100 - Cloud/AWS/Solutions Architect Associate/VPC|VPC (SAA)]]: CIDR blocks, subnets, IGW, NAT Gateways (Regional), Bastion Hosts, NACL, ephemeral ports, Security Groups vs NACL, Default VPC
- [[100 - Cloud/AWS/Solutions Architect Associate/ELB|ELB (SAA)]]: ALB, NLB, GWLB, target groups, routing, cross-zone load balancing
- [[100 - Cloud/AWS/Solutions Architect Associate/Route 53|Route 53 (SAA)]]: record types, hosted zones, TTL, CNAME vs Alias, routing policies, health checks
- [[100 - Cloud/AWS/Solutions Architect Associate/CloudFront|CloudFront (SAA)]]: origins (S3, VPC, Custom), geo restriction, cache invalidations, CloudFront vs S3 CRR
- [[100 - Cloud/AWS/Solutions Architect Associate/Global Accelerator|Global Accelerator (SAA)]]: Anycast IP, health checks, failover, vs CloudFront comparison
- [[Connectivity]]: VPN, Direct Connect, Transit Gateway, PrivateLink (cross-VPC/on-prem)
- [[Messaging]]: EventBridge, SQS, SNS (async decoupling)
- [[100 - Cloud/AWS/Solutions Architect Associate/Messaging|Messaging (SAA)]]: sync vs async patterns, SQS Standard Queue attributes

## Data + AI

- [[Database]]: RDS, Aurora, DynamoDB, DocumentDB, Neptune, ElastiCache, Redshift
- [[100 - Cloud/AWS/Solutions Architect Associate/RDS|RDS (SAA)]]: Read Replicas vs Multi-AZ, RDS Custom, Backup & Restore, RDS Proxy, Encryption & Security, Aurora (Serverless, Global DB, ML, Babelfish, Fast Cloning)
- [[100 - Cloud/AWS/Solutions Architect Associate/Databases|Databases (SAA)]]: DocumentDB (MongoDB-compatible, serverless, global)
- [[100 - Cloud/AWS/Solutions Architect Associate/ElastiCache|ElastiCache (SAA)]]: Redis vs Memcached, caching strategies, clustering, high availability
- [[Analytics]]: Kinesis, Glue, EMR, Athena, QuickSight, OpenSearch
- [[100 - Cloud/AWS/Solutions Architect Associate/Analytics|Analytics (SAA)]]: Glue (ETL, DataBrew, Studio, Streaming), Lake Formation (data lake governance), Flink (stream processing), MSK (managed Kafka), Athena (serverless SQL, federated query), Redshift (data warehousing, Spectrum, loading data), OpenSearch (search, dashboards, log analysis), EMR (Hadoop, Spark, big data), QuickSight (BI, dashboards, SPICE), Big Data Ingestion Pipeline
- [[AI]]: pre-built AI services, SageMaker, Bedrock, Amazon Q
- [[100 - Cloud/AWS/Solutions Architect Associate/AI and ML|AI & ML (SAA)]]: Rekognition (content moderation, A2I), Transcribe (PII redaction, language ID), Polly (SSML, lexicons), Lex & Connect, Comprehend & Comprehend Medical (PHI), SageMaker (build/train/deploy), Kendra (document search), Personalize (recommendations), Textract (document extraction)

## Security + governance

- [[100 - Cloud/AWS/Cloud Practitioner/Security]]: IAM, Identity Center, Secrets Manager, Systems Manager
- [[Governance]]: Organizations, Control Tower, Config, Audit Manager
- [[Monitoring]]: CloudWatch, CloudTrail, Trusted Advisor
- [[100 - Cloud/AWS/Solutions Architect Associate/IAM|IAM (SAA)]]: policy evaluation logic, condition keys, resource-based policies vs roles, permission boundaries, Organizations (SCPs, tag policies), Control Tower guardrails, IAM Identity Center (permission sets, ABAC), Directory Services
- [[100 - Cloud/AWS/Solutions Architect Associate/KMS|KMS (SAA)]]: key types (symmetric/asymmetric, AWS owned/managed/customer), key policies, key rotation, multi-region keys, client-side encryption (DynamoDB/Aurora Global), S3 replication encryption, KMS vs CloudHSM comparison
- [[100 - Cloud/AWS/Solutions Architect Associate/Security Services|Security Services (SAA)]]: SSM Parameter Store (Standard vs Advanced, parameter policies), Secrets Manager (rotation, multi-region secrets), ACM (public/private certs, DNS validation, importing certs)
- [[100 - Cloud/AWS/Solutions Architect Associate/API Gateway|API Gateway (SAA)]]: endpoint types (Edge-Optimized, Regional, Private), ACM integration for custom domain names
- [[100 - Cloud/AWS/Solutions Architect Associate/WAF Shield Firewall Manager|WAF, Shield, Firewall Manager (SAA)]]: WAF (Web ACL rules, IP sets, rate-based rules, deployment targets), Shield (Standard vs Advanced, DDoS protection), Firewall Manager (organization-wide security policies)
- [[100 - Cloud/AWS/Solutions Architect Associate/Threat Detection|Threat Detection (SAA)]]: GuardDuty (threat discovery, ML, data sources, EventBridge), Inspector (EC2/ECR/Lambda vulnerability scanning, network reachability), Macie (sensitive data discovery, PII)
- [[100 - Cloud/AWS/Solutions Architect Associate/DDoS Best Practices|DDoS Best Practices (SAA)]]: Mitigation techniques (edge services, SYN cookies), EC2 instance selection (enhanced networking), Auto Scaling (BP7), ELB configuration (BP6, connection tracking), Shield Advanced benefits
- [[100 - Cloud/AWS/Solutions Architect Associate/Monitoring|Monitoring (SAA)]]: CloudWatch (metrics, logs, alarms, EventBridge, Insights), CloudTrail (events, Insights, retention), AWS Config (rules, remediations, notifications), comparison

## Foundational

- [[Global Infrastructure]]: Regions, AZs, edge, Outposts
- [[CloudFormation]]: IaC, stacks, CDK, SAM
- [[WellArchitected]]: six pillars
- [[CloudAdoptionFramework]]: CAF, 7 Rs

## Operations

- [[Billing]] · [[Support]] · [[Marketplace]] · [[SpecializedServices]]

## Cross-domain links

- [[Containers#Amazon EKS: Elastic Kubernetes Service|EKS]] → [[Cluster]]: managed Kubernetes control plane
- [[Compute]] (Lambda) ↔ event-driven [[Messaging]] (EventBridge, SNS triggers)

---

← [[README]]
