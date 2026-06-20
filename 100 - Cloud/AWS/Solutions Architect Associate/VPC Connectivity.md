---
domain: aws
track: solutions-architect-associate
topic: networking
type: note
tags:
  - aws
  - solutions-architect-associate
  - networking
  - vpc
  - connectivity
  - peering
  - endpoints
  - vpn
  - direct-connect
  - transit-gateway
---

# VPC Connectivity

Connectivity options for linking VPCs to each other, to on-premises networks, and to AWS services.

## VPC Peering

Privately connect two VPCs using AWS' network, making them behave as if they were in the same network.

**Key characteristics:**
- Must not have overlapping CIDRs
- VPC Peering connection is NOT transitive (must be established for each VPC that needs to communicate with one another)
- Must update route tables in each VPC's subnets to ensure EC2 instances can communicate

**Cross-account and cross-region:**
- Can create VPC Peering connections between VPCs in different AWS accounts or regions
- Can reference a security group in a peered VPC (works cross-accounts, same region)

**Use cases:**
- Multi-account architectures with shared services
- Mergers and acquisitions requiring network integration
- Microservices split across VPCs

> For foundational peering concepts, see [[100 - Cloud/AWS/Cloud Practitioner/Connectivity#VPC Peering|Cloud Practitioner: VPC Peering]].

## VPC Endpoints (AWS PrivateLink)

Connect to AWS services using a private network instead of the public internet. Endpoints are redundant and scale horizontally. They remove the need for IGW, NAT Gateway, etc. to access AWS services.

**Troubleshooting:**
- Check DNS Resolution setting in your VPC
- Check Route Tables

### Types of Endpoints

| Type | Mechanism | Supported Services | Cost | Security Groups |
|------|-----------|-------------------|------|-----------------|
| **Interface Endpoints** | Provisions an ENI (private IP) as entry point | Most AWS services | $ per hour + $ per GB | Yes (must attach) |
| **Gateway Endpoints** | Provisions a gateway, used as target in route table | S3 and DynamoDB only | Free | No |

**Exam tip:** Gateway endpoints are preferred when available (free). Interface endpoints are preferred when access is required from on-premises (Site-to-Site VPN or Direct Connect), a different VPC, or a different region.

**Example - DynamoDB access from Lambda:**
- **Option 1 (public):** Lambda in VPC needs NAT Gateway in public subnet + Internet Gateway
- **Option 2 (better & free):** Deploy a VPC Gateway Endpoint for DynamoDB + update Route Tables

> For foundational PrivateLink concepts, see [[100 - Cloud/AWS/Cloud Practitioner/Connectivity#AWS PrivateLink|Cloud Practitioner: PrivateLink]].

## VPC Flow Logs

Capture information about IP traffic going into and out of network interfaces.

**Scope levels:**
- VPC Flow Logs (all interfaces in VPC)
- Subnet Flow Logs (all interfaces in subnet)
- Elastic Network Interface (ENI) Flow Logs (specific interface)

**Use cases:**
- Monitor and troubleshoot connectivity issues
- Analytics on usage patterns
- Detect malicious behavior

**Destinations:** S3, CloudWatch Logs, Kinesis Data Firehose

**Captures traffic from AWS managed interfaces:** ELB, RDS, ElastiCache, Redshift, WorkSpaces, NAT Gateway, Transit Gateway

### Flow Log Syntax

| Field | Purpose |
|-------|---------|
| srcaddr & dstaddr | Identify problematic IP addresses |
| srcport & dstport | Identify problematic ports |
| Action | Success or failure due to Security Group / NACL |

**Query options:** Athena on S3 or CloudWatch Logs Insights

### Troubleshooting Security Groups & NACL Issues

Look at the **ACTION** field:

**Incoming Requests:**
- Inbound REJECT → NACL or Security Group blocking
- Inbound ACCEPT, Outbound REJECT → NACL blocking return traffic

**Outgoing Requests:**
- Outbound REJECT → NACL or Security Group blocking
- Outbound ACCEPT, Inbound REJECT → NACL blocking return traffic

### CloudWatch Permissions

IAM Service Role associated with VPC Flow Logs must have permissions to publish logs to CloudWatch Logs:
- `logs:CreateLogGroup`
- `logs:CreateLogStream`
- `logs:PutLogEvents`

## Site-to-Site VPN

Secure connection from on-premises network to AWS VPC over the public internet.

**Components:**
- **Virtual Private Gateway (VGW):** VPN concentrator on AWS side, attached to the VPC. Can customize ASN (Autonomous System Number)
- **Customer Gateway (CGW):** Software application or physical device on customer side

**Setup requirements:**
- Public internet-routable IP for Customer Gateway device
- If behind NAT device with NAT traversal (NAT-T), use the public IP of the NAT device
- Enable **Route Propagation** for the Virtual Private Gateway in the route table associated with your subnets
- Add ICMP protocol to inbound security group rules if you need to ping EC2 instances from on-premises

### AWS VPN CloudHub

Secure communication between multiple sites using multiple VPN connections.

**Characteristics:**
- Low-cost hub-and-spoke model for primary or secondary network connectivity
- VPN-only (goes over public internet)
- Setup: connect multiple VPN connections on the same VGW, enable dynamic routing, configure route tables

> For foundational VPN concepts, see [[100 - Cloud/AWS/Cloud Practitioner/Connectivity#AWS Site-to-Site VPN|Cloud Practitioner: Site-to-Site VPN]].

## Direct Connect (DX)

Dedicated private connection from on-premises data center to AWS VPC.

**Characteristics:**
- Dedicated connection between your data center and AWS Direct Connect locations
- Requires Virtual Private Gateway on your VPC
- Access both public resources (S3) and private (EC2) on same connection
- Supports IPv4 and IPv6

**Use cases:**
- Increase bandwidth throughput for large data sets (lower cost than internet)
- Consistent network experience for real-time data feeds
- Hybrid environments (on-premises + cloud)

### Connection Types

| Type | Speed Range | Setup | Notes |
|------|-------------|-------|-------|
| **Dedicated** | 1 Gbps to 400 Gbps | Request to AWS, completed by Direct Connect Partners | Physical ethernet port dedicated to customer |
| **Hosted** | 50 Mbps to 25 Gbps | Via Direct Connect Partners | Capacity added/removed on demand, lead times often >1 month |

### Direct Connect Gateway

Required when setting up Direct Connect to one or more VPCs in **different regions** (same account).

### Encryption

- Data in transit is **not encrypted** but is private
- **Direct Connect + VPN** provides IPsec-encrypted private connection (extra security, slightly more complex)

### Resiliency

**High Resiliency:** One connection at multiple locations

**Maximum Resiliency:** Separate connections terminating on separate devices in more than one location

**Backup options:**
- Backup Direct Connect connection (expensive)
- Site-to-Site VPN connection as backup

> For foundational Direct Connect concepts, see [[100 - Cloud/AWS/Cloud Practitioner/Connectivity#AWS Direct Connect|Cloud Practitioner: Direct Connect]].

## Transit Gateway

Hub-and-spoke (star) connection for transitive peering between thousands of VPCs and on-premises networks.

**Characteristics:**
- Regional resource, can work cross-region
- Share cross-account using Resource Access Manager (RAM)
- Can peer Transit Gateways across regions
- Route Tables limit which VPCs can communicate with other VPCs
- Works with Direct Connect Gateway and VPN connections
- Supports IP Multicast (not supported by any other AWS service)

### ECMP (Equal-Cost Multi-Path)

Routing strategy to forward packets over multiple best paths.

**Use case:** Create multiple Site-to-Site VPN connections to increase bandwidth to AWS.

**Throughput comparison:**

| Scenario | VPN Connections | Throughput | Notes |
|----------|----------------|------------|-------|
| VPN to Virtual Private Gateway | 1x (2 tunnels) | 1.25 Gbps | Single VPC |
| VPN to Transit Gateway | 1x (2 tunnels) | 2.5 Gbps | ECMP, multiple VPCs |
| VPN to Transit Gateway | 2x | 5.0 Gbps | ECMP |
| VPN to Transit Gateway | 3x | 7.5 Gbps | ECMP |

Transit Gateway provides higher throughput with ECMP and scales linearly with additional VPN connections. Additional cost applies per GB of TGW processed data.

### Share Direct Connect Between Multiple Accounts

Use AWS Resource Access Manager (RAM) to share Transit Gateway with other accounts, enabling shared Direct Connect connectivity.

> For foundational Transit Gateway concepts, see [[100 - Cloud/AWS/Cloud Practitioner/Connectivity#AWS Transit Gateway|Cloud Practitioner: Transit Gateway]].

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
