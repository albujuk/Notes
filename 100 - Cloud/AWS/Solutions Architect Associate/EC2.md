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
---

# EC2 — Placement Groups

To meet the needs of your workload, you can launch a group of _interdependent_ EC2 instances into a **placement group** to influence their placement.

## Placement Strategies

| Strategy | Behavior | Use case |
|----------|----------|----------|
| **Cluster** | Packs instances close together inside an AZ | Low-latency, tightly-coupled HPC workloads |
| **Partition** | Spreads instances across logical partitions (no shared hardware between partitions) | Large distributed/replicated workloads (Hadoop, Cassandra, Kafka) |
| **Spread** | Strictly places a small group across distinct underlying hardware | Reducing correlated failures |

Placement groups are **optional**. Without one, EC2 tries to spread instances across hardware to minimize correlated failures.

## Pricing

No charge for creating a placement group.

## Rules and Limitations

- An instance can be in **one placement group at a time** — can't span multiple groups
- You **can't merge** placement groups
- On-Demand Capacity Reservations and zonal Reserved Instances can be used with placement groups (capacity is auto-matched)
- **Dedicated Hosts** can't be launched in placement groups
- **Spot Instances** configured to stop or hibernate on interruption can't be launched in placement groups

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]