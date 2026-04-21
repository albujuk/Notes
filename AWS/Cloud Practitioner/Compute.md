# Compute

## EC2 — Elastic Compute Cloud

EC2 is AWS's core compute service. Provides **virtual machines (instances)** running on AWS-managed physical hardware.

### Multi-Tenancy

Instances run on **shared physical hardware** but are fully isolated — this model is called **multi-tenancy**. AWS's hypervisor enforces isolation so one tenant cannot access another's data or resources.

### Elasticity (Scaling)

EC2 instances are resizable; pay only for what you use, adjust capacity any time:

| Direction | Action | When to use |
|-----------|--------|-------------|
| Scale **up** | Increase instance size (more CPU/RAM) | Single workload needs more power |
| Scale **down** | Decrease instance size | Over-provisioned; reduce cost |
| Scale **out** | Add more instances (horizontal) | Distribute load across many instances |
| Scale **in** | Remove instances | Demand has dropped |

---

## Cloud vs. On-Premises

| Benefit | Cloud (AWS) | On-Premises |
|---------|-------------|-------------|
| Upfront cost | None — pay-as-you-go | Large capital expenditure for hardware |
| Provisioning speed | Seconds to minutes | Weeks to months (procurement + setup) |
| Hardware management | AWS-managed | Your team's responsibility |

---

## Other Compute Services

### AWS Lambda

**Serverless compute** — run code without provisioning or managing servers. Pay only for compute time consumed.

- Upload your function code; Lambda handles everything else (scaling, availability, patching)
- Triggered by events (HTTP requests via API Gateway, S3 uploads, DynamoDB changes, etc.)
- **Max execution duration: 15 minutes** per invocation — not suited for long-running processes

**Cold starts** — on the first invocation (or after a period of idle), Lambda must initialize a new execution environment (download code, start runtime). This adds latency. Mitigation: **Provisioned Concurrency** keeps a set number of environments pre-warmed and ready.

**Concurrency** — each simultaneous invocation runs in its own isolated environment. Default soft limit: **1,000 concurrent executions per region** (can be raised via support request).

**Pricing** — charged on two dimensions:
- **Requests:** first 1 M requests/month free; $0.20 per 1 M requests after
- **Duration:** billed in GB-seconds (memory allocated × execution time); 400,000 GB-seconds/month free

**When Lambda is unsuitable:**

| Scenario | Reason |
|----------|--------|
| Workloads > 15 min | Hard execution limit |
| Persistent connections (e.g., WebSockets, long-poll DB) | Environment torn down after each invocation |
| Stateful applications requiring in-memory state across calls | Each invocation is isolated; no shared state |

### Elastic Beanstalk

Fully managed PaaS. Deploy and scale web applications without managing infrastructure. You provide the code; Beanstalk handles capacity, load balancing, and health monitoring.

### AWS Batch

Fully managed service for large-scale **batch computing jobs**. Dynamically provisions the optimal compute resources based on job requirements.

### Amazon Lightsail

Simplified cloud platform offering VPS, containers, and databases with **predictable pricing**. Designed for simpler workloads and developers new to AWS.

### AWS Outposts

Extends AWS infrastructure and services to **on-premises locations** for low-latency or local data processing requirements.

---

← [[AWS/Cloud Practitioner/Index]] · [[Home]]
