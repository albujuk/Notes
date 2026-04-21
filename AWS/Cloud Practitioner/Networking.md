# Networking

## VPC (Virtual Private Cloud)

Isolated virtual network within AWS. You define the IP range (CIDR block), subnets, route tables, and gateways. Resources ([[Compute#EC2|EC2]], RDS, etc.) launch inside a VPC. Each AWS account gets a default VPC per [[Global Infrastructure#Regions|region]].

## Subnets

Subdivisions of a VPC's IP range, scoped to one AZ.

- **Public subnet** — has route to [[#Internet Gateway|internet gateway]]. Resources here can be internet-facing (e.g. load balancers, bastion hosts).
- **Private subnet** — no route to internet gateway. Resources here are isolated (e.g. databases, app servers). Can still reach internet via [[#NAT Gateway]] for outbound-only traffic.

## Internet Gateway

Attaches to a VPC to allow traffic between VPC resources and the public internet. Without it, nothing in the VPC can reach the internet.

## NAT Gateway

Sits in a public subnet. Lets resources in private subnets initiate outbound internet traffic (e.g. download updates) without being reachable from the internet. Managed by AWS — no patching needed.

## Route Tables

Control where subnet traffic is directed. Each subnet associates with one route table. Public subnets have a route `0.0.0.0/0 → internet gateway`; private subnets route `0.0.0.0/0 → NAT gateway`. Each [[Global Infrastructure#Availability Zones (AZs)|AZ]] gets its own subnet.

## Security Groups

Stateful firewall at the **instance** level. You define allow rules only (no explicit deny). Return traffic automatically allowed. Default: deny all inbound, allow all outbound.

## Network ACLs (NACLs)

Stateful**less** firewall at the **subnet** level. Rules evaluated in number order; first match wins. Must explicitly allow both inbound and return traffic. Default NACL allows all traffic.

| | Security Group | NACL |
|--|--|--|
| Level | Instance | Subnet |
| State | Stateful | Stateless |
| Rules | Allow only | Allow + Deny |
| Evaluation | All rules | In order, first match |

## Virtual Private Gateway

VPN endpoint on the AWS side. Attach to a VPC to enable encrypted connections from on-premises networks or remote clients into that VPC.

---

## Connecting to AWS

### AWS Client VPN

Remote workers connect to AWS or on-premises resources over an encrypted VPN tunnel from their device. Client-based, OpenVPN-compatible.

### AWS Site-to-Site VPN

Links an entire on-premises network to a VPC over an encrypted IPsec tunnel through the internet. Uses a Virtual Private Gateway on the AWS side and a customer gateway on the on-premises side.

### AWS PrivateLink

Exposes services privately inside AWS without traffic touching the public internet. Uses VPC endpoints — traffic stays on the AWS network. Common for accessing AWS services (S3, DynamoDB) or sharing your own service with other VPCs/accounts.

### AWS Direct Connect

Dedicated physical network connection from on-premises to AWS. Bypasses the public internet entirely — lower latency, more consistent throughput. Takes weeks to provision; higher cost than VPN.

---

| Option | Path | Use case |
|--------|------|----------|
| Client VPN | Internet (encrypted) | Remote workers |
| Site-to-Site VPN | Internet (encrypted) | On-prem ↔ VPC |
| PrivateLink | AWS network | Private service access |
| Direct Connect | Dedicated line | High-throughput / low-latency on-prem link |

---

## DNS — Route 53

AWS managed DNS service.

- Routes users to endpoints (EC2, ELB, S3, CloudFront, on-prem)
- Supports routing policies: **Simple**, **Weighted** (A/B split), **Latency-based**, **Failover**, **Geolocation**
- Can register domain names

## CDN — CloudFront

Content delivery network. Caches content at [[Global Infrastructure#Edge Locations|edge locations]] (Points of Presence) close to users — reduces latency for static assets, video, APIs.

- Integrates with S3, [[Compute#EC2|EC2]], ELB, [[#DNS — Route 53|Route 53]]
- DDoS protection via AWS Shield (Standard included free)
- Can restrict content by geography

---

← [[AWS/Cloud Practitioner/Index]] · [[Home]]
