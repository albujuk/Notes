# AWS Global Infrastructure

## Regions

Geographic area containing multiple data centers. Each region is isolated — data doesn't leave unless you move it.

- Currently 30+ regions worldwide
- Named by geography: `us-east-1`, `eu-west-2`, `ap-southeast-1`
- When choosing a region, consider:
  - **Compliance** — data sovereignty laws (data must stay in country)
  - **Latency** — proximity to users
  - **Feature availability** — not all services launch in all regions simultaneously
  - **Pricing** — varies by region

---

## Availability Zones (AZs)

One or more discrete data centers inside a region, each with redundant power, networking, and connectivity.

- Each region has **2–6 AZs** (usually 3)
- AZs are physically separate but connected via low-latency links
- Named by appending a letter: `us-east-1a`, `us-east-1b`, `us-east-1c`

**Why it matters:** deploying across multiple AZs = fault tolerance. One AZ fails, others keep running.

```
Region: us-east-1
├── AZ: us-east-1a  ← data center(s)
├── AZ: us-east-1b
└── AZ: us-east-1c
```

---

## Edge Locations

Data centers used by **[[Networking#CDN — CloudFront|CloudFront]]** (CDN) and other edge services to cache and deliver content close to users.

- 400+ edge locations globally — far more than regions
- Cache content from origin servers (S3, [[Compute#EC2 — Elastic Compute Cloud|EC2]], etc.)
- Reduce latency by serving from nearest edge node
- Also used by: **[[Networking#DNS — Route 53|Route 53]]** (DNS), **AWS Shield**, **AWS WAF**

**Edge location vs Region:**

| | Region | Edge Location |
|---|---|---|
| Purpose | Run AWS services | Cache / deliver content |
| Count | ~30 | 400+ |
| AZs inside | Yes | No |
| Example service | [[Compute#EC2 — Elastic Compute Cloud\|EC2]], RDS | [[Networking#CDN — CloudFront\|CloudFront]], [[Networking#DNS — Route 53\|Route 53]] |

---

## Local Zones

Extensions of a [[#Regions|region]] placed closer to dense population centers. Low-latency access to select AWS services ([[Compute#EC2 — Elastic Compute Cloud|EC2]], EBS, RDS).

- Not a full region — subset of services only
- Use case: latency-sensitive apps (gaming, media, ML inference)

---

## Outposts

AWS-managed hardware rack installed **in your own data center**. Runs native AWS services on-premises.

- Same APIs, tools, and hardware as AWS cloud
- Useful for data residency requirements or workloads that must stay on-prem
- Covered in [[AWS/Cloud Practitioner/Compute]] under EC2 section

---

← [[AWS/Cloud Practitioner/Index]] · [[Home]]