# Kubernetes — Index

Learning path in order:

| # | Topic | File | What's inside |
|---|-------|------|---------------|
| 1 | Cluster Architecture | [[Intro]] | Nodes, Master vs Worker, Components (API Server, etcd, kubelet, Scheduler, Controller), cluster diagram |
| 2 | Pods | [[Pods]] | Pod concept, kubectl commands, multi-container pods, sidecar pattern (emptyDir, fluentd example) |
| 3 | Workloads | [[Workloads]] | ReplicationController, ReplicaSet, RC vs RS comparison, scaling commands |

---

## Quick kubectl Reference

```bash
# Pods
kubectl run nginx --image=nginx --dry-run=client -o yaml
kubectl apply -f <file>.yaml
kubectl get pods -o wide
kubectl describe pod <name>

# ReplicaSet
kubectl get rs
kubectl scale rs <name> --replicas=<n>

# ReplicationController
kubectl get rc
```

---

← Start: [[Intro]] · [[Pods]] · [[Workloads]]
