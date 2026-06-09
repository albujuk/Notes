---
domain: aws
track: solutions-architect-associate
topic: networking
type: note
tags:
  - aws
  - solutions-architect-associate
  - networking
  - route53
  - dns
---

# Route 53

AWS managed DNS service. Translates domain names to IP addresses and routes traffic to AWS resources (EC2, ELB, S3, CloudFront, on-prem).

See [[100 - Cloud/AWS/Cloud Practitioner/Networking#DNS: Route 53|Networking: Route 53]] for the high-level overview.

## Record Types

| Type | Maps | Notes |
|------|------|-------|
| **A** | hostname → IPv4 | Most common record type |
| **AAAA** | hostname → IPv6 | Same as A but for IPv6 |
| **CNAME** | hostname → another hostname | Target must have an A or AAAA record. Can't use on Zone Apex |
| **NS** | domain → Name Servers | Controls how traffic is routed for a domain |

**Zone Apex restriction:** CNAME can't be created for the top node of a DNS namespace (e.g. `example.com`). Works for subdomains (e.g. `www.example.com`). Use an Alias record instead.

## Hosted Zones

A container for records that define how to route traffic to a domain and its subdomains.

| Type | Scope | Example |
|------|-------|---------|
| **Public** | Internet-facing | `app.mypublicdomain.com` |
| **Private** | Within one or more VPCs | `app.company.internal` |

Cost: **$0.50 per month** per hosted zone.

## TTL (Time To Live)

The duration a DNS record is cached before it must be re-fetched. Lower TTL = faster propagation but more queries (and cost). Higher TTL = fewer queries but slower changes.

## CNAME vs Alias

AWS resources (ELB, CloudFront, S3 website) expose an AWS hostname like `lb1-1234.us-east-2.elb.amazonaws.com`. To map `myapp.mydomain.com` to that:

| | CNAME | Alias |
|---|---|---|
| **Points to** | Any hostname | AWS resource only |
| **Zone Apex** | Not supported | Supported |
| **Cost** | Standard DNS query pricing | Free |
| **TTL** | Configurable by user | Managed by AWS (not configurable) |
| **Health checks** | No | Native health check |
| **Use when** | Pointing to non-AWS or non-root domain | Pointing to AWS resources (always prefer over CNAME for AWS targets) |

**Rule of thumb:** if the target is an AWS resource, always use an Alias record.

## Routing Policies

How Route 53 responds to DNS queries. Don't confuse with [[ELB|load balancer]] routing.

### Simple

- Routes traffic to a single resource
- Can specify multiple values in the same record; if multiple values are returned, a random one is chosen by the **client**
- When Alias is enabled, can only specify one AWS resource
- Cannot be associated with Health Checks

### Weighted

- Controls the % of requests that go to each resource by assigning relative weights
- DNS records must have the same name and type
- Can be associated with Health Checks
- Use cases: load balancing between regions, A/B testing, blue/green deployments
- Weight of 0 stops traffic to that resource; if all weights are 0, all records are returned equally

### Latency

- Routes to the resource with the least latency close to users
- Latency is based on traffic between users and AWS Regions
- Can be associated with Health Checks (failover capability)

### Failover

- Active/passive failover: primary resource serves traffic until health check fails, then DNS fails over to secondary

### Geolocation

- Routes based on the geographic location of the user making the DNS query
- Useful for serving locale-specific content or restricting distribution

### Geoproximity

- Routes based on geographic location of users **and** resources, with optional bias to expand/shrink regions

### IP-based

- Routes based on the IP address the query originates from (client IP)

### Multi-Value

- Like Simple, but Route 53 returns multiple healthy records (up to 8) and the client picks one
- Can be associated with Health Checks

## Health Checks

Automated DNS failover based on endpoint health. Route 53 health checkers around the world send requests to your endpoint every 10 or 30 seconds.

**Three types:**

| Type | Monitors | Use case |
|------|----------|----------|
| **Endpoint** | An application, server, or AWS resource | Public endpoints |
| **Calculated** | Other health checks | Composite health logic |
| **CloudWatch Alarm** | A CloudWatch alarm | Private resources (DynamoDB throttles, RDS, custom metrics) |

**How health is determined:**

- Each health checker evaluates **response time** and whether the endpoint fails to respond
- An endpoint is considered unhealthy when it fails a configurable number of consecutive checks (failure threshold)
- Route 53 aggregates data globally: if **>18%** of health checkers report healthy, the endpoint is healthy; if **<=18%**, it's unhealthy
- The 18% threshold ensures multiple regions agree, preventing false negatives from network isolation

Health Checks integrate with CloudWatch metrics.

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]] · [[100 - Cloud/AWS/Solutions Architect Associate/ELB|ELB]]