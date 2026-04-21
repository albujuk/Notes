# Containers

Containers package code + dependencies together, ensuring consistent runs across environments. AWS offers managed services for storing, orchestrating, and running containers.

---

## Amazon ECR — Elastic Container Registry

Fully managed **container image registry**. Store, manage, and deploy container images securely at scale. Integrates with ECS and EKS.

---

## Amazon ECS — Elastic Container Service

Fully managed **container orchestration** service. Deploys, manages, and scales containerized applications on AWS. No need to manage your own orchestration infrastructure.

---

## Amazon EKS — Elastic Kubernetes Service

Fully managed **Kubernetes** service. Runs Kubernetes clusters on AWS and on-premises. Automates infrastructure management and integrates with AWS networking, security, and storage.

> Use ECS for AWS-native simplicity. Use EKS when you need standard Kubernetes or portability.

---

## AWS Fargate

**Serverless compute engine** for containers. Run containers without managing servers or clusters. Works with both ECS and EKS.

| | ECS / EKS on EC2 | ECS / EKS on Fargate |
|-|------------------|----------------------|
| Server management | You manage EC2 instances | AWS manages all servers |
| Control | More control | Less control |
| Use case | Predictable, large workloads | Variable, hands-off ops |

---

← [[AWS/Cloud Practitioner/Index]] · [[Home]]
