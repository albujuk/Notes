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

## Deployment

The controller you should use for all stateless workloads. A Deployment manages [[#ReplicaSet|ReplicaSets]] for you — when you create a Deployment, it creates a ReplicaSet under the hood. The real power comes when you update your application.

**How a rolling update works:**
1. You apply a new image version (`kubectl set image` or `kubectl apply` with updated YAML).
2. Deployment creates a **new ReplicaSet** with the updated pod template.
3. It scales the new RS up one pod at a time while scaling the old RS down.
4. The old RS is kept at 0 replicas — this is what enables rollback.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
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
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

```bash
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get deployment nginx-deployment
kubectl describe deployment nginx-deployment

# Update image (triggers rolling update)
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1

# Watch rollout progress
kubectl rollout status deployment/nginx-deployment

# Rollback to previous version
kubectl rollout undo deployment/nginx-deployment

# View rollout history
kubectl rollout history deployment/nginx-deployment

# Scale
kubectl scale deployment nginx-deployment --replicas=5

# Generate YAML via dry-run
kubectl create deployment nginx --image=nginx --replicas=3 --dry-run=client -o yaml
```

---

## ReplicaSet vs Deployment

| Feature | ReplicaSet | Deployment |
| :--- | :--- | :--- |
| **Maintains pod count** | Yes | Yes (via RS) |
| **Rolling updates** | No | Yes |
| **Rollback** | No | Yes (`kubectl rollout undo`) |
| **Update strategy** | None | `RollingUpdate` / `Recreate` |
| **Revision history** | No | Yes (old RSes kept) |
| **Use directly in prod?** | No | Yes |

Rule: always use Deployments. A bare ReplicaSet has no update or rollback capability. Think of the ReplicaSet as the engine and the Deployment as the car — you always drive the car.

---

← [[300 - Containers/Kubernetes/README|Kubernetes]] · [[Home]]
