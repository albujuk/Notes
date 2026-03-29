# Kubernetes Introduction

## Node
A **Node** is a physical or virtual machine with Kubernetes installed. Nodes serve as the worker machines in a cluster and are sometimes referred to as "minions."

## Cluster
A **Cluster** is a collection of nodes grouped together. Distributing workloads across multiple nodes ensures high availability and efficient load balancing.

## Master Node
The **Master Node** is a specialized node responsible for managing and orchestrating the other nodes within the cluster, ensuring the desired state of the system is maintained.

---
# Kubernetes Components
1. API Server
   acts as the front end for k8s (users, management devices, clis, and so on talks to it to intract witht the cluster)
2. etcd
   is a distributed key-value store used by k8s to store data
   e.g. with multi nodes and multi masters it stores all the data on the nodes in distributed matter.
   it implements lock within the cluster to insure there is no conflicts between the masters
3. kubelet
4. container run time
5. controller
6. scheduler
   responsable for distirbuting the work or containers across multible nodes
7. 