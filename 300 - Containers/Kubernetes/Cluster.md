---
domain: kubernetes
track: core
topic: architecture
type: note
tags:
  - kubernetes
  - architecture
  - nodes
  - master-node
  - worker-node
  - eks
---

# Cluster Architecture

## Node
A **Node** is a physical or virtual machine with Kubernetes installed. Nodes serve as the worker machines in a cluster and are sometimes referred to as "minions."

## Cluster
A **Cluster** is a collection of nodes grouped together. Distributing workloads across multiple nodes ensures high availability and efficient load balancing.

## Master Node
The **Master Node** is a specialized node responsible for managing and orchestrating the other nodes within the cluster, ensuring the desired state of the system is maintained.

## Master vs. Worker Nodes

| Feature                | Master Node (Control Plane)                                                | Worker Node (Data Plane)                                           |
| :--------------------- | :------------------------------------------------------------------------- | :----------------------------------------------------------------- |
| **Role**               | Management & Orchestration                                                 | Execution & Workload                                               |
| **Responsibility**     | Maintaining desired state, scheduling, and cluster-wide decisions.         | Running application containers and managing local networking.      |
| **Primary Components** | API Server, etcd, Scheduler, Controller Manager, Cloud Controller Manager. | Kubelet, Kube-proxy, Container Runtime (e.g., Docker, containerd). |
| **Count**              | 1 (Single) or 3+ (High Availability).                                      | 1 to many (Scalable).                                              |

> Component details: [[Components]]

---

## Managed Kubernetes on AWS

AWS offers **[[Containers#Amazon EKS — Elastic Kubernetes Service|EKS]]** — a fully managed control plane so you don't run the master node yourself.

---

← [[300 - Containers/Kubernetes/README|Kubernetes]]
