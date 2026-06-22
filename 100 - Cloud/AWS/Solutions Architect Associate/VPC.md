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
  - subnets
  - nat
  - nacl
  - ipv6
  - traffic-mirroring
  - network-firewall
  - data-transfer-costs
---

# VPC: Virtual Private Cloud

Isolated virtual network within AWS where you launch your resources. Logically isolated from other virtual networks in your AWS account.

**Why use VPC:**
- Control over IP address ranges, subnets, route tables, and network gateways
- Secure network architecture with public and private subnets
- Fine-grained access control with security groups and NACLs
- Connectivity to on-premises networks via VPN or Direct Connect
- VPC peering and Transit Gateway for multi-VPC architectures

> For foundational VPC concepts, see [[100 - Cloud/AWS/Cloud Practitioner/Networking#VPC (Virtual Private Cloud)|Cloud Practitioner: VPC]].

## VPC Components

### CIDR Blocks

Each VPC has one or more CIDR blocks (IP address ranges). You can have up to **5 CIDR blocks per VPC** (soft limit).

**Allowed private IPv4 ranges:**
- 10.0.0.0/8 (10.0.0.0 – 10.255.255.255)
- 172.16.0.0/12 (172.16.0.0 – 172.31.255.255)
- 192.168.0.0/16 (192.168.0.0 – 192.168.255.255)

**Size constraints:**
- Minimum: /28 (16 IP addresses)
- Maximum: /16 (65,536 IP addresses)

**Important:** Your VPC CIDR should NOT overlap with your other networks (e.g., corporate on-premises network) to avoid routing conflicts when setting up hybrid connectivity.

### Subnets

Subdivide your VPC into smaller segments. Each subnet is tied to **one Availability Zone** and cannot span multiple AZs.

**Public subnet:** Has a route to an Internet Gateway (IGW). Resources here can have public IP addresses and communicate directly with the internet.

**Private subnet:** No direct route to the internet. Resources here use NAT Gateways or NAT Instances for outbound internet access.

**Route tables:** Each subnet is associated with a route table that determines where network traffic is directed. You can have a main route table (default) and custom route tables.

### Internet Gateway (IGW)

Enables resources in a VPC to connect to the internet. Horizontally scaled, highly available, and redundant by design.

**Key points:**
- One VPC can only be attached to one IGW and vice versa
- Must be created separately from the VPC
- IGW alone does not allow internet access; you must also edit route tables to route traffic (0.0.0.0/0) to the IGW

### NAT Gateways

Allows EC2 instances in **private subnets** to connect to the internet (for updates, patches, etc.) while preventing inbound connections from the internet.

**Characteristics:**
- AWS-managed service, highly available within a single AZ
- Created in a specific AZ with an Elastic IP
- Scales automatically up to 100 Gbps
- No security groups to manage
- Pay per hour + data processing charges
- Cannot be used by instances in the same subnet (only from other subnets)
- Requires an IGW for outbound internet access (Private Subnet → NAT Gateway → IGW)

**Multi-AZ setup:** NAT Gateway is resilient within a single AZ. For fault tolerance across AZ failures, create a NAT Gateway in each AZ and configure route tables accordingly.

**Regional NAT Gateway:** Highly available NAT Gateway that spans multiple AZs automatically. Has its own route tables, eliminating per-AZ deployments. Automatically expands to new AZs when resources are launched.

### NAT Instances (Legacy)

EC2 instances configured to perform NAT. Launched in public subnets.

**Requirements:**
- Disable source/destination check on the EC2 instance
- Attach an Elastic IP
- Configure route tables to route private subnet traffic to the NAT instance

**Drawbacks:**
- Not highly available out of the box (requires Auto Scaling Group + failover scripts)
- Bandwidth depends on EC2 instance type
- You manage security groups, OS patches, and maintenance
- Reached end of standard support on December 31, 2020

**Use as Bastion Host:** NAT instances can double as bastion hosts for SSH access to private instances. NAT Gateways cannot.

### NAT Gateway vs NAT Instance

| Attribute        | NAT Gateway                                                                  | NAT Instance                                           |
| ---------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------ |
| Availability     | Highly available per AZ; create one per AZ for zone-independent architecture | Requires failover scripts (e.g., ASG + custom scripts) |
| Bandwidth        | Scales up to 100 Gbps                                                        | Depends on instance type                               |
| Maintenance      | Managed by AWS                                                               | You manage OS patches, software updates                |
| Performance      | Optimized for NAT traffic                                                    | Generic AMI configured for NAT                         |
| Cost             | Per NAT gateway + duration + data processed                                  | Per instance + duration + instance type/size           |
| Type and size    | Uniform offering, no sizing decisions                                        | Must choose instance type/size based on workload       |
| Public IP        | Associate Elastic IP at creation                                             | Elastic IP or public IP; can swap anytime              |
| Private IP       | Auto-selected from subnet CIDR                                               | Manually assigned from subnet CIDR                     |
| Security groups  | Cannot attach SGs to NAT Gateway; use SGs on resources behind it             | Attach SGs directly to the NAT instance                |
| Port forwarding  | Not supported                                                                | Can manually configure                                 |
| Bastion host     | Not supported                                                                | Can be used as bastion host                            |
| Timeout behavior | Sends RST on timeout                                                         | Sends FIN on timeout                                   |
| IP fragmentation | UDP only; TCP and ICMP fragments dropped                                     | Reassembles fragments for UDP, TCP, and ICMP           |
| Status           | Current, recommended                                                         | Legacy; end of standard support Dec 31, 2020           |

### Bastion Hosts

EC2 instance in a public subnet used as a jump box to SSH into private EC2 instances.

**Security group configuration:**
- Bastion Host SG: Allow inbound SSH (port 22) from restricted CIDR (e.g., corporate public IP range)
- Private EC2 SG: Allow inbound SSH from Bastion Host's security group or private IP

**Best practice:** Use Systems Manager Session Manager instead of bastion hosts for secure, auditable shell access without managing SSH keys or opening inbound ports.

## Network Access Control Lists (NACL)

Stateless firewall at the subnet level. Controls traffic in and out of subnets.

**Characteristics:**
- One NACL per subnet (new subnets get the Default NACL)
- Rules have numbers (1-32766); lower number = higher precedence
- First matching rule wins
- Last rule is an asterisk (*) that denies all traffic (implicit deny)
- Newly created NACLs deny everything by default
- Support both allow and deny rules
- Stateless: return traffic must be explicitly allowed

**Default NACL:** Accepts all inbound and outbound traffic for associated subnets. Do not modify it; create custom NACLs instead.

**Use case:** Block specific IP addresses at the subnet level.

### Ephemeral Ports

When a client initiates a connection, it uses a random ephemeral port for the response. NACLs must allow these ports for return traffic.

**Common ephemeral port ranges:**
- Linux kernels (including Amazon Linux): 32768-61000
- Windows Server 2008+: 49152-65535
- NAT Gateway: 1024-65535
- Elastic Load Balancing: 1024-65535
- AWS Lambda: 1024-65535

**Rule of thumb:** Open ephemeral ports 1024-65535 to cover most client types. Place deny rules for malicious ports before the allow rule for the wide range.

## Security Groups vs NACL

| Aspect | Security Group | NACL |
|--------|----------------|------|
| **Operates at** | Instance level | Subnet level |
| **Rule types** | Allow rules only | Allow and deny rules |
| **State** | Stateful (return traffic automatically allowed) | Stateless (return traffic must be explicitly allowed) |
| **Rule evaluation** | All rules evaluated before decision | Rules evaluated in order (lowest to highest), first match wins |
| **Application** | Applied to EC2 instance when specified | Automatically applies to all instances in the subnet |

**Best practice:** Use security groups for most use cases. Use NACLs when you need to block specific IPs at the subnet level.

## Default VPC

All new AWS accounts get a default VPC in each region.

**Characteristics:**
- New EC2 instances launch into the default VPC if no subnet is specified
- Has internet connectivity (attached IGW)
- All EC2 instances get public IPv4 addresses
- Provides public and private IPv4 DNS names

**Why it's not ideal for production:**
- Limited control over IP ranges and network architecture
- All instances have public IPs (security concern)
- Single CIDR block (no room for growth)
- Not suitable for multi-tier architectures with public/private subnets

**Best practice:** Create custom VPCs with proper subnet tiering (public, private, data) for production workloads.

## IPv6 in VPC

IPv6 is the successor to IPv4, providing 3.4 x 10^38 unique addresses (vs IPv4's 4.3 billion).

**Key differences from IPv4:**
- Format: 8 groups of hexadecimal values separated by colons (e.g., `2001:db8:3333:4444:5555:6666:7777:8888`)
- Zero compression: consecutive zero groups can be collapsed to `::` (e.g., `2001:db8::1234:5678`)
- **Every IPv6 address in AWS is public and internet-routable** (no private IPv6 range exists)

**IPv6 in your VPC:**
- IPv4 cannot be disabled for VPCs and subnets
- IPv6 can be enabled to operate in **dual-stack mode** (both IPv4 and IPv6)
- EC2 instances get at least a private internal IPv4 and a public IPv6
- Instances can communicate using either IPv4 or IPv6 to the internet through an Internet Gateway

## Egress-only Internet Gateway

Like a NAT Gateway, but for **IPv6 only**.

**Characteristics:**
- Allows instances in your VPC to make outbound connections over IPv6 while preventing the internet from initiating inbound IPv6 connections
- You must update the Route Tables to route IPv6 traffic (e.g., `::/0`) to the Egress-only Internet Gateway

**Comparison:**

| Feature | NAT Gateway | Egress-only Internet Gateway |
|---------|-------------|------------------------------|
| IP version | IPv4 | IPv6 |
| Direction | Outbound only | Outbound only |
| Cost | Per hour + per GB | No additional cost |

## VPC Traffic Mirroring

Capture and inspect network traffic in your VPC for security analysis and troubleshooting.

**Components:**
- **Source:** ENIs (Elastic Network Interfaces) to capture traffic from
- **Target:** ENI or Network Load Balancer to send captured traffic to

**Characteristics:**
- Route traffic to security appliances that you manage
- Capture all packets or filter packets of interest (optionally truncate packets)
- Source and Target can be in the same VPC or different VPCs (via VPC Peering)

**Use cases:** content inspection, threat monitoring, troubleshooting, compliance auditing

## AWS Network Firewall

Managed firewall service that protects your entire VPC from Layer 3 through Layer 7.

**Capabilities:**
- Inspects traffic in any direction: VPC-to-VPC, outbound to internet, inbound from internet, to/from Direct Connect and Site-to-Site VPN
- Internally uses AWS Gateway Load Balancer
- Rules can be centrally managed cross-account by [[100 - Cloud/AWS/Solutions Architect Associate/WAF Shield Firewall Manager|Firewall Manager]] to apply to many VPCs

**Fine-grained controls:**
- Supports 1000s of rules with IP & port filtering (e.g., 10,000s of IPs)
- Protocol filtering (e.g., block SMB protocol for outbound)
- Stateful domain list rule groups (e.g., only allow outbound to `*.mycorp.com`)
- General pattern matching using regex
- Traffic actions: allow, drop, or alert
- Active flow inspection with intrusion-prevention capabilities (IPS)
- Logs rule matches to S3, CloudWatch Logs, or Kinesis Data Firehose

## Networking Costs in AWS

### Data Transfer Pricing (per GB)

| Scenario | Cost |
|----------|------|
| Traffic within same AZ (private IP) | Free |
| Traffic across AZs (private IP) | $0.01 |
| Traffic across AZs (public IP / Elastic IP) | $0.02 |
| Inter-region traffic | $0.02 |

**Cost optimization tips:**
- Use **private IPs** instead of public IPs for better savings and network performance
- Keep traffic within the same AZ for maximum savings (at the cost of high availability)
- Keep as much internet traffic within AWS as possible to minimize costs

### Key Examples

**S3 data transfer:**
- S3 ingress: free
- S3 to internet: $0.09/GB
- S3 to CloudFront: free ($0.00/GB)
- CloudFront to internet: $0.085/GB (slightly cheaper than S3 direct, plus caching and 7x cheaper S3 request pricing)
- S3 Transfer Acceleration: +$0.04 to $0.08/GB on top of data transfer pricing (50-500% faster)
- S3 Cross-Region Replication: $0.02/GB

**EC2 in private subnet accessing S3:**
- NAT Gateway: $0.045/hour + $0.045/GB data processed
- Cross-region S3: $0.09/GB
- Same-region S3 via Gateway Endpoint: free (no cost for Gateway Endpoint, $0.01/GB same-region transfer)

**Direct Connect:** Co-located Direct Connect locations in the same AWS Region result in lower egress network costs.

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
