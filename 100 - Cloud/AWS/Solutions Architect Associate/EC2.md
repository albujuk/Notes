---
domain: aws
track: solutions-architect-associate
topic: compute
type: note
tags:
  - aws
  - solutions-architect-associate
  - compute
  - ec2
  - placement-groups
  - eni
  - hibernation
---

# EC2

## Placement Groups

To meet the needs of your workload, you can launch a group of _interdependent_ EC2 instances into a **placement group** to influence their placement.

### Placement Strategies

| Strategy | Behavior | Use case |
|----------|----------|----------|
| **Cluster** | Packs instances close together inside an AZ | Low-latency, tightly-coupled HPC workloads |
| **Partition** | Spreads instances across logical partitions (no shared hardware between partitions) | Large distributed/replicated workloads (Hadoop, Cassandra, Kafka) |
| **Spread** | Strictly places a small group across distinct underlying hardware | Reducing correlated failures |

Placement groups are **optional**. Without one, EC2 tries to spread instances across hardware to minimize correlated failures.

### Pricing

No charge for creating a placement group.

### Rules and Limitations

- An instance can be in **one placement group at a time**; can't span multiple groups
- You **can't merge** placement groups
- On-Demand Capacity Reservations and zonal Reserved Instances can be used with placement groups (capacity is auto-matched)
- **Dedicated Hosts** can't be launched in placement groups
- **Spot Instances** configured to stop or hibernate on interruption can't be launched in placement groups

---

## Elastic Network Interfaces (ENI)

An ENI is a logical networking component in a VPC that represents a virtual network card. You can create and configure network interfaces and attach them to instances in the same Availability Zone.

Attributes follow the ENI as it's attached/detached and reattached to another instance; network traffic redirects with it.

### Attributes

- Primary private IPv4 address (from subnet range)
- Primary IPv6 address
- Secondary private IPv4 addresses
- One Elastic IP per private IPv4 address
- One public IPv4 address
- Secondary IPv6 addresses
- Security groups
- MAC address
- Source/destination check flag
- Description

### Primary vs Secondary

- **Primary network interface**: default ENI on every instance; cannot be detached.
- **Secondary network interfaces**: additional ENIs you create and attach; max count varies by instance type.

---

## Hibernation

Hibernation saves RAM contents to the EBS root volume before stopping the instance. On resume:

- EBS root volume restored to previous state
- RAM contents reloaded
- Running processes resumed
- Data volumes reattached; instance ID retained

### Prerequisites

| Requirement | Detail |
|-------------|--------|
| **RAM size** | Linux < 150 GiB; Windows ≤ 16 GiB (T3/T3a Windows: ≥ 1 GiB recommended) |
| **Root volume type** | Must be EBS (not instance store) |
| **Root volume size** | Large enough for RAM + OS/apps; space reserved at launch |
| **Root volume encryption** | Mandatory: RAM data is always encrypted when written to EBS |

Instance must be enabled for hibernation at launch and meet all [hibernation prerequisites](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/hibernating-prerequisites.html).

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]