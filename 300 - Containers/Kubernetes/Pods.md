---
domain: kubernetes
track: core
topic: pods
type: note
tags:
  - kubernetes
  - pods
  - containers
---

# Pods

A **Pod** is the smallest deployable unit in Kubernetes — a wrapper around one or more containers that share the same network namespace and storage. Every container in Kubernetes runs inside a Pod. Pods are managed at scale by [[Controllers|workload controllers]] (ReplicaSet, Deployment).

- Each Pod gets its own internal IP address within the cluster.
- Pods are ephemeral: if they die, Kubernetes spins up a new one (with a new IP).
- Kubernetes scales by adding/removing Pods, not containers directly.

## kubectl Commands
> Full reference: [[kubectl]]

```bash
# Create pod (imperative)
kubectl run nginx --image=nginx

# Generate YAML without creating (dry run)
kubectl run nginx --image=nginx --dry-run=client -o yaml

# Generate YAML and save to file
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml

# Apply manifest
kubectl apply -f pod.yaml

# List pods
kubectl get pods
kubectl get pods -o wide          # includes node, IP

# Inspect
kubectl describe pod nginx
kubectl logs nginx

# Delete
kubectl delete pod nginx
```

---

## Multi-Container Pods

A Pod can hold multiple containers that share localhost and volumes. Used for helper patterns like sidecars (log shippers, proxies, config reloaders).

> Full pattern + sidecar example: [[Patterns]]

---

← [[300 - Containers/Kubernetes/README|Kubernetes]]
