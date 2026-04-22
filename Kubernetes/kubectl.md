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

> Deployment, Service, and ConfigMap commands are previews — full docs not yet written. See [[Kubernetes/missing]].

```bash
# Pods — [[Pods]]
kubectl run nginx --image=nginx --dry-run=client -o yaml
kubectl apply -f <file>.yaml
kubectl get pods -o wide
kubectl describe pod <name>

# ReplicaSet — [[Workloads#ReplicaSet]]
kubectl get rs
kubectl scale rs <name> --replicas=<n>

# ReplicationController — [[Workloads#ReplicationController]]
kubectl get rc

# Deployment
kubectl create deployment <name> --image=<image> --dry-run=client -o yaml
kubectl get deployments
kubectl describe deployment <name>
kubectl rollout status deployment/<name>
kubectl rollout undo deployment/<name>
kubectl set image deployment/<name> <container>=<image>

# Service
kubectl expose deployment <name> --port=<port> --type=NodePort
kubectl get svc
kubectl describe svc <name>

# ConfigMap
kubectl create configmap <name> --from-literal=<key>=<value>
kubectl get configmaps
kubectl describe configmap <name>
```

---

← [[Index]] · [[Home]]
