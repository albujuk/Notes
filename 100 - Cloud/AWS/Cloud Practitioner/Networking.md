---
domain: aws
track: cloud-practitioner
topic: networking
type: note
tags:
  - aws
  - cloud-practitioner
  - networking
  - vpc
  - route53
  - cloudfront
  - security-groups
  - nacl
  - api-gateway
---

# Networking

## VPC (Virtual Private Cloud)

Isolated virtual network within AWS. You define the IP range (CIDR block), subnets, route tables, and gateways. Resources ([[Compute#EC2|EC2]], RDS, etc.) launch inside a VPC. Each AWS account gets a default VPC per [[Global Infrastructure#Regions|region]].

## Subnets

Subdivisions of a VPC's IP range, scoped to one AZ.

- **Public subnet:** has route to [[#Internet Gateway|internet gateway]]. Resources here can be internet-facing (e.g. load balancers, bastion hosts).
- **Private subnet:** no route to internet gateway. Resources here are isolated (e.g. databases, app servers). Can still reach internet via [[#NAT Gateway]] for outbound-only traffic.

## Internet Gateway

Attaches to a VPC to allow traffic between VPC resources and the public internet. Without it, nothing in the VPC can reach the internet.

## NAT Gateway

Sits in a public subnet. Lets resources in private subnets initiate outbound internet traffic (e.g. download updates) without being reachable from the internet. Managed by AWS: no patching needed.

## Route Tables

Control where subnet traffic is directed. Each subnet associates with one route table. Public subnets have a route `0.0.0.0/0 → internet gateway`; private subnets route `0.0.0.0/0 → NAT gateway`. Each [[Global Infrastructure#Availability Zones (AZs)|AZ]] gets its own subnet.

## Security Groups

Stateful firewall at the **instance** level. You define allow rules only (no explicit deny). Return traffic automatically allowed. Default: deny all inbound, allow all outbound.

## Network ACLs (NACLs)

Stateless firewall at the **subnet** level. Rules evaluated in number order; first match wins. Must explicitly allow both inbound and return traffic. Default NACL allows all traffic.

| Feature            | Security Groups                                                       | Network ACLs                                                 |
| ------------------ | --------------------------------------------------------------------- | ------------------------------------------------------------ |
| **Scope**          | Instance level (attached to EC2 instances)                            | Subnet level (associated with subnets)                       |
| **State**          | Stateful (remembers state)                                            | Stateless (doesn't remember state)                           |
| **Rule types**     | Only allow type rules                                                 | Both allow and deny type rules                               |
| **Return traffic** | Return traffic is automatically allowed if inbound traffic is allowed | Return traffic must be implicitly allowed in both directions |
| **Uses**           | Fine-grained control of traffic for individual EC2 instances          | Broad control of traffic in and out of subnets               |

> Securing subnets and resources with NACLs and security groups is **your responsibility** under the Shared Responsibility Model: networking traffic protection is a critical defense for applications *in* the cloud.

## Virtual Private Gateway

VPN endpoint on the AWS side. Attach to a VPC to enable encrypted connections from on-premises networks or remote clients into that VPC. Used by [[Connectivity#AWS Site-to-Site VPN|Site-to-Site VPN]].

---

## DNS: Route 53

AWS managed DNS service.

- Routes users to endpoints (EC2, ELB, S3, CloudFront, on-prem)
- Supports routing policies: **Simple**, **Weighted** (A/B split), **Latency-based**, **Failover**, **Geolocation**
- Can register domain names

## CDN: CloudFront

Content delivery network. Caches content at [[Global Infrastructure#Edge Locations|edge locations]] (Points of Presence) close to users: reduces latency for static assets, video, APIs.

- Integrates with S3, [[Compute#EC2|EC2]], ELB, [[#DNS: Route 53|Route 53]]
- DDoS protection via AWS Shield (Standard included free)
- Can restrict content by geography

## Global Accelerator

Routes application traffic through the AWS private global network instead of the public internet: faster, more reliable, avoids congested internet paths.

- Assigns two static anycast IPs to your application (no DNS propagation delays on failover)
- Intelligent traffic routing + fast failover across [[Global Infrastructure#Regions|regions]] and [[Global Infrastructure#Availability Zones (AZs)|AZs]]
- Works with EC2, ALB, NLB, Elastic IPs

| | CloudFront | Global Accelerator |
|---|---|---|
| **What it optimizes** | Content delivery (caches at edge) | Network path (no caching) |
| **Best for** | Static assets, video, HTTP cacheable content | TCP/UDP apps, APIs, gaming, IoT |
| **Entry point** | Edge location caches | Anycast IPs → AWS network |

## API Gateway

Managed service for creating, publishing, and securing APIs at any scale. Acts as the front door for backend services (Lambda, EC2, HTTP endpoints).

- Handles auth, throttling, caching, request/response transformation
- Supports REST, HTTP, and WebSocket APIs
- Scales automatically: no infrastructure to manage

---

→ [docs.aws.amazon.com: VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)

← [[100 - Cloud/AWS/Cloud Practitioner/README|Cloud Practitioner]] · [[Connectivity]] →
