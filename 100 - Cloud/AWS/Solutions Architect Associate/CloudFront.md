---
domain: aws
track: solutions-architect-associate
topic: networking
type: note
tags:
  - aws
  - solutions-architect-associate
  - networking
  - cloudfront
  - cdn
---

# CloudFront

AWS Content Delivery Network (CDN). Distributes content globally via edge locations (Points of Presence) to improve read performance and user experience.

See [[100 - Cloud/AWS/Cloud Practitioner/Networking#CDN: CloudFront|Networking: CloudFront]] for the high-level overview.

Integrates with [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Shield|Shield]] and [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS WAF (Web Application Firewall)|WAF]] for DDoS protection.

## Origins

| Origin Type | Use Case |
|-------------|----------|
| **S3 bucket** | Distribute files, cache at edge. Upload to S3 through CloudFront. Secured using Origin Access Control (OAC) |
| **VPC Origin** | Applications in VPC private subnets: private ALB, NLB, or EC2 instances |
| **Custom Origin (HTTP)** | S3 website (must enable static website hosting), public ALB, any public HTTP backend |

## VPC Origins

Deliver content from applications hosted in VPC private subnets without exposing them to the internet.

Supported targets:
- Application Load Balancer (private)
- Network Load Balancer (private)
- EC2 Instances (private)

## Geo Restriction

Restrict who can access your distribution based on geographic location (determined by a 3rd-party Geo-IP database):

- **Allowlist**: only users in approved countries can access content
- **Blocklist**: users in banned countries are blocked

Use case: copyright laws, content licensing by region.

## Cache Invalidations

When the origin is updated, CloudFront only gets refreshed content after the TTL expires. To force a refresh (bypassing TTL), perform a **cache invalidation**:

- Invalidate all files (`*`) or a specific path (`/images/*`)
- Invalidations take effect within minutes
- Alternative: use versioned filenames to avoid invalidation altogether

## CloudFront vs S3 Cross-Region Replication

| | CloudFront | S3 Cross-Region Replication |
|---|---|---|
| **Scope** | Global edge network | Specific regions (must configure each) |
| **Caching** | Cached for a TTL (e.g. a day) | Near real-time updates |
| **Access** | Read and write | Read only |
| **Best for** | Static content available everywhere | Dynamic content at low latency in a few regions |

## Traffic Spike Resilience

CloudFront in front of an ALB improves application resilience to periodic spikes in request rates:

- Caches content at edge locations (POPs) globally, reducing origin load
- Regional edge caches bring content closer to viewers even when not popular enough for POPs
- Origin failover supports data resiliency
- Scales automatically to handle traffic spikes without provisioning

**Use case:** Multi-tier applications with predictable or unpredictable traffic spikes. CloudFront absorbs read traffic at the edge, preventing origin overload.

**Exam tip:** When asked about making applications resilient to traffic spikes, CloudFront is preferred over Global Accelerator. GA focuses on static IPs and regional failover for non-HTTP or specific HTTP use cases — it doesn't cache, so the origin still bears the full spike load.

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]] · [[100 - Cloud/AWS/Solutions Architect Associate/Global Accelerator|Global Accelerator]] · [[100 - Cloud/AWS/Solutions Architect Associate/ELB|ELB]]
