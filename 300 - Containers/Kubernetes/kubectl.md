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

## `--save-config` and the last-applied annotation

`kubectl create --save-config` writes `kubectl.kubernetes.io/last-applied-configuration` on the object: the full JSON of the current spec. Without it, a later `apply` has no baseline and may diff incorrectly.

```yaml
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"apps/v1","kind":"Deployment",...}
```

Use `--save-config` on `create` when you plan to manage the object with `apply` later. See [[Concepts#Imperative vs Declarative|Concepts]] for the broader create vs apply distinction.

---

```bash
# Explain: resource field docs
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
# args after -- become the container command
kubectl create deployment <name> --image=<image> -- <cmd> <arg1> <arg2>

# Update
kubectl set image deployment/<name> <container>=<image>
kubectl set image deployment/<name> <container>=<image> --record  # record cause in rollout history

# Rollout
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout undo deployment/<name>
kubectl rollout undo deployment/<name> --to-revision=<n>

# Scale
kubectl scale deployment <name> --replicas=<n>
```

---

→ [kubernetes.io: kubectl Reference](https://kubernetes.io/docs/reference/kubectl/)

← [[300 - Containers/Kubernetes/README|Kubernetes]]
