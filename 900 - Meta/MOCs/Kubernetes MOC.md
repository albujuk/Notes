---
type: moc
tags:
  - moc
  - kubernetes
---

# Kubernetes MOC

How the Kubernetes notes connect. The folder index ([[300 - Containers/Kubernetes/README|Kubernetes README]]) lists files in learning order — this MOC groups them by concept.

## Control plane vs data plane

- [[Cluster]] — what nodes are, master vs worker split
- [[Components]] — API Server, etcd, scheduler, controller (master); kubelet, kube-proxy, container runtime (worker)

The control plane decides; the data plane runs.

## Workload abstractions

Stack from atom up:

- [[Pods]] — smallest deployable unit, one or more containers sharing localhost + storage
- [[Controllers]] — keep N pod replicas alive (ReplicationController → ReplicaSet → [[Controllers#Deployment|Deployment]])
  - [[Controllers#Deployment|Deployment]] wraps ReplicaSet and adds rolling updates + rollback — use this for all stateless workloads
- [[Patterns]] — multi-container Pod patterns (sidecar, log shipper)

A Pod alone is fragile. Wrap it in a controller for self-healing replicas. Use a Deployment (not a bare ReplicaSet) so you can update and roll back without downtime.

## Cross-domain links

- [[Cluster]] → [[Containers#Amazon EKS — Elastic Kubernetes Service|EKS]] — AWS managed control plane (you don't run master nodes)
- [[Components]] → [[Containers#Amazon ECS — Elastic Container Service|ECS]] — AWS-native alternative when Kubernetes portability not needed

## Operating reference

- [[kubectl]] — command reference
- [[300 - Containers/Kubernetes/missing|missing]] — topics not yet documented (Services, Ingress, ConfigMaps, Secrets, PVs)

---

← [[README]]
