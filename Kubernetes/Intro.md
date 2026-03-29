# Kubernetes Introduction

## Node
A **Node** is a physical or virtual machine with Kubernetes installed. Nodes serve as the worker machines in a cluster and are sometimes referred to as "minions."

## Cluster
A **Cluster** is a collection of nodes grouped together. Distributing workloads across multiple nodes ensures high availability and efficient load balancing.

## Master Node
The **Master Node** is a specialized node responsible for managing and orchestrating the other nodes within the cluster, ensuring the desired state of the system is maintained.

## Master vs. Worker Nodes

| Feature | Master Node | Worker Node |
| :--- | :--- | :--- |
| **Role** | Control Plane (Management) | Data Plane (Execution) |
| **Responsibility** | Orchestration, scheduling, and cluster management. | Running application containers (Pods). |
| **Key Components** | API Server, etcd, Scheduler, Controller Manager. | Kubelet, Container Runtime, Kube-proxy. |
| **Count** | Typically 1 or 3 (for high availability). | Scalable to hundreds or thousands. |

---

# Kubernetes Components

## 1. API Server
The **API Server** acts as the front end for Kubernetes. Users, management devices, and command-line interfaces (CLIs) interact with it to manage the cluster.

## 2. etcd
**etcd** is a distributed key-value store used by Kubernetes to store all cluster data. It maintains information across multiple nodes and masters, implementing locks to ensure there are no configuration conflicts between master components.

## 3. kubelet
The **kubelet** is an agent that runs on each node in the cluster. It is responsible for ensuring that containers are running as expected within their pods.

## 4. Container Runtime
The **Container Runtime** is the underlying software used to run containers. It handles the actual execution of containerized applications on the nodes.

## 5. Controller
The **Controller** is the "brain" behind orchestration. It monitors nodes and containers, making decisions to bring up new instances if the current state deviates from the desired state (e.g., if a node or container goes down).

## 6. Scheduler
The **Scheduler** is responsible for distributing workloads and containers across multiple nodes, ensuring efficient resource utilization and adherence to placement constraints.

