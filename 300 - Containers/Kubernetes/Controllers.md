---
domain: kubernetes
track: core
topic: controllers
type: note
tags:
  - kubernetes
  - workloads
  - controllers
  - replicaset
  - replication-controller
  - deployment
  - scaling
---

# Workload Controllers

Controllers manage [[Pods|Pods]] at scale — keeping a desired number of replicas alive and rescheduling them when they die. **ReplicationController** was the original; **ReplicaSet** replaced it; **Deployment** wraps ReplicaSet and is what you actually use day-to-day.

## ReplicationController

Older API object. Ensures a specified number of [[Pods|Pod]] replicas are running at all times. If a Pod dies, RC creates a replacement. Replaced by [[#ReplicaSet]] (and Deployment) but still supported.

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
```

```bash
kubectl apply -f rc.yaml
kubectl get rc
kubectl get rc nginx-rc
kubectl describe rc nginx-rc
kubectl delete rc nginx-rc
```

---

## ReplicaSet

Successor to [[#ReplicationController]]. Key difference: supports **set-based selectors** (`In`, `NotIn`, `Exists`) vs RC's equality-only selectors. In practice, you rarely create ReplicaSets directly — Deployments manage them for you.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
```

```bash
kubectl apply -f rs.yaml
kubectl get rs
kubectl get rs nginx-rs
kubectl describe rs nginx-rs

# Scale
kubectl scale rs nginx-rs --replicas=5
kubectl scale rs nginx-rs --replicas=1

# Delete RS but keep pods
kubectl delete rs nginx-rs --cascade=orphan

# Delete RS and pods
kubectl delete rs nginx-rs
```

### Generate YAML via dry-run

```bash
# No direct `kubectl create replicaset` — use a Deployment dry-run and strip the extra fields
kubectl create deployment nginx --image=nginx --replicas=3 --dry-run=client -o yaml
```

---

## RC vs ReplicaSet

| Feature             | ReplicationController          | ReplicaSet                                   |
| :------------------ | :----------------------------- | :------------------------------------------- |
| **API version**     | `v1`                           | `apps/v1`                                    |
| **Selector type**   | Equality only (`key: value`)   | Set-based (`matchLabels`/`matchExpressions`) |
| **Status**          | Legacy                         | Current (but use Deployment)                 |
| **Managed by**      | Standalone                     | Deployment                                   |

---

← [[300 - Containers/Kubernetes/README|Kubernetes]] · [[Home]]
