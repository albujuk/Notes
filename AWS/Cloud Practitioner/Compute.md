# Compute

## EC2 - Elastic Compute Cloud

EC2 is AWS's core compute service. It provides **virtual machines (instances)** that run on AWS-managed physical hardware.

### Multi-Tenancy

EC2 instances run on **shared physical hardware** but are fully isolated from one another 4 this model is called **multi-tenancy**. AWS's hypervisor enforces the isolation so that one tenant cannot access another's data or resources.

### Elasticity (Scaling)

EC2 instances are resizable, you pay only for what you use and can adjust capacity at any time:

| Direction | Action | When to use |
|-----------|--------|-------------|
| Scale **up** | Increase instance size (more CPU/RAM) | Single workload needs more power |
| Scale **down** | Decrease instance size | Over-provisioned; reduce cost |
| Scale **out** | Add more instances (horizontal) | Distribute load across many instances |
| Scale **in** | Remove instances | Demand has dropped |

---

## Cloud vs. On-Premises

| Benefit | Cloud (AWS) | On-Premises |
|---------|-------------|-------------|
| Upfront cost | None - pay-as-you-go | Large capital expenditure for hardware |
| Provisioning speed | Seconds to minutes | Weeks to months (procurement + setup) |
| Hardware management | AWS-managed | Your team's responsibility |

