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

See [[100 - Cloud/AWS/Cloud Practitioner/Networking#DNS: Route 53|Networking: Route 53]] for the high-level overview and routing policies.

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

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]] · [[100 - Cloud/AWS/Solutions Architect Associate/ELB|ELB]]