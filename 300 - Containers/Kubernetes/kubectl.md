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
```

---

## Pods — [[Pods|Pods]]

```bash
kubectl run <name> --image=<image>
kubectl run <name> --image=<image> --dry-run=client -o yaml
kubectl apply -f <file>.yaml
kubectl get pods
kubectl get pods -o wide
kubectl describe pod <name>
kubectl delete pod <name>
kubectl logs <name>
kubectl exec -it <name> -- /bin/sh
```

---

## ReplicationController — [[Controllers#ReplicationController|Controllers]]

```bash
kubectl apply -f rc.yaml
kubectl get rc
kubectl get rc <name>
kubectl describe rc <name>
kubectl delete rc <name>
```

---

## ReplicaSet — [[Controllers#ReplicaSet|Controllers]]

```bash
kubectl apply -f rs.yaml
kubectl get rs
kubectl get rs <name>
kubectl describe rs <name>
kubectl scale rs <name> --replicas=<n>
kubectl delete rs <name> --cascade=orphan   # delete RS, keep pods
kubectl delete rs <name>                    # delete RS and pods
```

---

## Deployment — [[Controllers#Deployment|Controllers]]

```bash
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get deployment <name>
kubectl describe deployment <name>
kubectl delete deployment <name>

# Create / generate
kubectl create deployment <name> --image=<image> --replicas=<n>
kubectl create deployment <name> --image=<image> --replicas=<n> --dry-run=client -o yaml

# Update
kubectl set image deployment/<name> <container>=<image>

# Rollout
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout undo deployment/<name>
kubectl rollout undo deployment/<name> --to-revision=<n>

# Scale
kubectl scale deployment <name> --replicas=<n>
```

---

← [[300 - Containers/Kubernetes/README|Kubernetes]] · [[Home]]
