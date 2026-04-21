# Containers

Containers package code + dependencies together, ensuring consistent runs across environments. AWS offers managed services for storing, orchestrating, and running containers.

---

## Amazon ECR — Elastic Container Registry

Fully managed **container image registry**. Store, manage, and deploy container images securely at scale. Integrates with [[#Amazon ECS|ECS]] and [[#Amazon EKS|EKS]].

- **Image layering:** stores images as layers; unchanged layers are shared across image versions, reducing storage and push/pull time.
- **Access control:** IAM-based — permissions are granted via IAM policies (no separate user management).
- **Lifecycle policies:** automate cleanup of old or untagged images to control storage costs.

---

## Amazon ECS — Elastic Container Service

Fully managed **container orchestration** service. Deploys, manages, and scales containerized applications on AWS. No need to manage your own orchestration infrastructure.

- **Task definition:** a JSON blueprint describing the container(s) to run — container image, CPU, memory, port mappings, environment variables, and IAM role.
- **Service:** runs and maintains a specified number (N) of copies of a task definition, restarting tasks that fail and integrating with load balancers.

---

## Amazon EKS — Elastic Kubernetes Service

Fully managed **[[Kubernetes/Intro|Kubernetes]]** service. Runs Kubernetes clusters on AWS and on-premises. Automates infrastructure management and integrates with AWS networking, security, and storage.

- Higher operational complexity than ECS; choose EKS when you need **Kubernetes portability** (e.g., multi-cloud strategy) or already run Kubernetes on-premises and want a consistent control plane.

**When to use:**

| | ECS | EKS |
|-|-----|-----|
| Best for | AWS-native simplicity, no K8s expertise needed | K8s portability, existing on-prem K8s workloads |
| Complexity | Lower | Higher |
| Ecosystem | AWS-specific | Standard Kubernetes tooling |

---

## AWS Fargate

**Serverless compute engine** for containers. Run containers without managing servers or clusters. Works with both [[#Amazon ECS|ECS]] and [[#Amazon EKS|EKS]].

| | ECS / EKS on [[Compute#EC2 — Elastic Compute Cloud\|EC2]] | ECS / EKS on Fargate |
|-|------------------|----------------------|
| Server management | You manage [[Compute#EC2 — Elastic Compute Cloud\|EC2]] instances | AWS manages all servers |
| Control | More control | Less control |
| Use case | Predictable, large workloads | Variable, hands-off ops |

- **Pricing:** billed per vCPU and memory per second — no idle EC2 costs, but per-unit cost is higher than EC2 for sustained workloads.
- **Cold start:** Fargate tasks have a short startup delay while the environment is provisioned; not ideal for latency-sensitive workloads requiring sub-second startup.

---

← [[AWS/Cloud Practitioner/Index]] · [[Home]]
