---
domain: kubernetes
track: core
topic: kubectl
type: note
tags:
  - kubernetes
  - kubectl
  - cli
  - reference
---

# kubectl Reference

```bash
# Explain — resource field docs
kubectl explain <resource>                  # top-level fields
kubectl explain <resource>.<field>          # drill into field (e.g. pod.spec.containers)
kubectl explain <resource> --recursive      # full field tree

# Pods — [[Workloads/Pods|Pods]]
kubectl run nginx --image=nginx --dry-run=client -o yaml
kubectl apply -f <file>.yaml
kubectl get pods -o wide
kubectl describe pod <name>

# ReplicaSet — [[Workloads/Controllers#ReplicaSet|Controllers]]
kubectl get rs
kubectl scale rs <name> --replicas=<n>

# ReplicationController — [[Workloads/Controllers#ReplicationController|Controllers]]
kubectl get rc
```

---

← [[README]] · [[Home]]
