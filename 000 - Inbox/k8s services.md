What it does?
It helps connects apps to other apps or users e.g. connect users with front-end with back-end and to the database.
this helps lose coupling between microservices 

## Service Types
**Node port:** It makes a pod accessible on a port in the node
**Cluster IP:** service creates a virtual IP (vip) inside the cluster to enable the communication between services e.g. set of frontend to set of backend
**Load balancer:** Creates a load balancer to distribute load across services

### NodePort
- The range of ports is (30000 - 32767)
```yaml
apiVersion: v1

kind: Service 

metadata:
  name: app-service

spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30080
```

```mermaid
graph LR
    User([User])

    subgraph Node
        direction TB
        NP[NodePort
        30080]
        subgraph Service
            SP[port: 80]
        end
        subgraph Pod
            TP[targetPort: 80]
        end
    end

    User -->|30080| NP
    NP --> SP
    SP --> TP
```

The mandatory field in the ports is port.
if targetPort is not present, it will have its value from port, and if NodePort is not  present it will pick a free port between 30000 and 32767

How do we connect or specify the desired pod? 
We use the labels, like what we did in replicasets rs

```yaml
apiVersion: v1

kind: Service 

metadata:
  name: app-service

spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30080
  selector:
    app: my-app
    type: front-end
```

The `selector` matches pod labels — Service routes traffic only to pods with **all** matching labels.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: my-app
    type: front-end
spec:
  containers:
    - name: my-container
      image: nginx
```

