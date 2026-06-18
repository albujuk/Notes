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
- [[100 - Cloud/AWS/Solutions Architect Associate/Analytics|Analytics (SAA)]]: Athena (serverless SQL, federated query), Redshift (data warehousing, Spectrum, snapshots), OpenSearch (search, dashboards), EMR (Hadoop, Spark, big data), QuickSight (BI, dashboards, SPICE)
- [[AI]]: pre-built AI services, SageMaker, Bedrock, Amazon Q

## Security + governance

- [[Security]]: IAM, Identity Center, Secrets Manager, Systems Manager
- [[Governance]]: Organizations, Control Tower, Config, Audit Manager
- [[Monitoring]]: CloudWatch, CloudTrail, Trusted Advisor

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
