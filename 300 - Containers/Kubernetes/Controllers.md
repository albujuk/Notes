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
```

→ full command reference: [[kubectl#ReplicationController|kubectl]]

---

## ReplicaSet

Successor to [[#ReplicationController]]. Key difference: supports **set-based selectors** (`In`, `NotIn`, `Exists`) vs RC's equality-only selectors. In practice, you rarely create ReplicaSets directly — Deployments manage them for you.

### Selector types

**RC — equality-only (`selector: key: value`):**
```yaml
selector:
  app: nginx        # matches pods where app == "nginx" exactly, nothing else
```

**RS — set-based (`selector: matchLabels` / `matchExpressions`):**

`matchLabels` is shorthand equality (same as RC):
```yaml
selector:
  matchLabels:
    app: nginx
```

`matchExpressions` is where RS gets powerful:
```yaml
selector:
  matchExpressions:
    - key: app
      operator: In
      values: [nginx, apache]     # app must be "nginx" OR "apache"

    - key: env
      operator: NotIn
      values: [prod]              # env must NOT be "prod"

    - key: tier
      operator: Exists            # "tier" key must exist, any value accepted
```

**Concrete example — RC limitation:**
You have pods labeled `app: nginx-v1` and `app: nginx-v2`. RC can only target one exact value, so you'd need two separate RCs. RS can target both with one selector: `app In (nginx-v1, nginx-v2)`.

**Why this matters for Deployments:** when a Deployment does a rolling update, it spins up a new RS for `nginx:1.16` while the old RS still owns `nginx:1.14` pods. The Deployment's selector uses `matchExpressions` to track pods across both RSes during the transition — impossible with RC's equality-only selectors.

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
```

→ full command reference: [[kubectl#ReplicaSet|kubectl]]

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

# Update image (triggers rolling update)
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1

# Rollback
kubectl rollout undo deployment/nginx-deployment
```

→ full command reference: [[kubectl#Deployment|kubectl]]

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

→ [kubernetes.io — Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

← [[300 - Containers/Kubernetes/README|Kubernetes]]
