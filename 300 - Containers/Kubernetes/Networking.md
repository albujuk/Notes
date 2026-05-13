---
domain: kubernetes
track: core
topic: networking
type: note
tags:
  - kubernetes
  - networking
  - cni
  - dns
  - ebpf
---

# Kubernetes Networking

Problem: Pod on Node A must reach Pod on Node B as if same LAN. OS knows nothing about Pods — network layer builds it.

## Network Namespaces

Each Pod = own Linux netns (own interfaces, routes, iptables). Containers in same Pod share one netns → talk via `localhost`, can't bind same port.

```
Node
├── Host netns (eth0, lo)
├── Pod A netns (eth0 @ 10.244.1.2)
├── Pod B netns (eth0 @ 10.244.1.3)
└── Pod C netns (eth0 @ 10.244.1.4)
```

**pause container**: hidden infra/sandbox container. Holds netns open so Pod IP stays stable when app containers restart. App + sidecars join its netns.

```mermaid
graph TD
    pause["pause container<br>owns the netns"] --> app["app container<br>joins pause's netns"]
    pause --> sidecar["sidecar<br>joins pause's netns"]
```

## Intra-Node

Pod netns ↔ host via **veth pair** (virtual cable). Host ends plug into Linux **bridge** (`cni0`/`cbr0`) = virtual switch. Same-node traffic never leaves host.

```mermaid
flowchart LR
    subgraph podA["Pod A netns"]
        ethA["eth0<br>10.244.1.2"]
    end
    subgraph host["Host netns"]
        bridge["bridge<br>(cni0 / cbr0)"]
        vethA["vethA"]
        vethB["vethB"]
    end
    subgraph podB["Pod B netns"]
        ethB["eth0<br>10.244.1.3"]
    end
    ethA <--> vethA
    vethA --- bridge
    bridge --- vethB
    vethB <--> ethB
```

## Inter-Node

Two approaches.

### Overlay (Encapsulation) — VXLAN

Wrap Pod packet in Node-to-Node UDP. Physical net sees only Node IPs.

```mermaid
sequenceDiagram
    participant PodA as Pod A<br>10.244.1.2
    participant NodeA as Node A
    participant PhysNet as Physical Network
    participant NodeB as Node B
    participant PodB as Pod B<br>10.244.2.3

    PodA->>NodeA: Original: src=10.244.1.2 dst=10.244.2.3
    NodeA->>NodeA: VXLAN wraps<br>src=NodeA_IP dst=NodeB_IP<br>UDP port 8472
    NodeA->>PhysNet: encapsulated packet
    PhysNet->>NodeB: Node-to-Node UDP traffic only
    NodeB->>NodeB: VXLAN decapsulates
    NodeB->>PodB: Original packet delivered
```

Works anywhere. Cost: ~50B header + CPU.

### Native Routing (No Overlay)

Each node owns Pod CIDR; program routes so traffic to that CIDR goes to that node IP.

```
Node A routes:
  10.244.1.0/24  via local bridge
  10.244.2.0/24  via 192.168.1.11  (Node B)
  10.244.3.0/24  via 192.168.1.12  (Node C)
```

Faster, no encap. Needs physical net to know routes — easy in cloud VPC, needs BGP on bare metal.

## CNI

Spec: binary kubelet calls on Pod start/stop. Must assign IP, set up interfaces, program routes.

```mermaid
flowchart TD
    A["kubelet creates Pod"] --> B["calls CNI binary<br>(e.g. /opt/cni/bin/flannel)"]
    B --> C1

    subgraph C["CNI plugin"]
        direction TB
        C1["Creates veth pair"] --> C2["Assigns IP from IPAM"]
        C2 --> C3["Connects veth to bridge"]
        C3 --> C4["Programs routes"]
    end

    C4 --> D["Pod has network"]
```

**IPAM** — hands out IPs:

| Plugin | How |
|---|---|
| `host-local` | Per-node fixed subnet |
| `dhcp` | DHCP server |
| `calico-ipam` | Calico block allocator |
| `whereabouts` | Cluster-wide (multus) |

## CNI Plugins

**Flannel** — simplest. VXLAN default, host-gw if L2 adjacent. No NetworkPolicy.

**Calico** — production standard. BGP native routing (fallback VXLAN/IP-in-IP). Full NetworkPolicy + CRD. WireGuard encryption. eBPF dataplane optional.

**Cilium** — eBPF-native. Replaces iptables + kube-proxy. L3/L4/L7 policy (HTTP, gRPC). Hubble observability. ClusterMesh. Needs kernel 5.10+.

**Weave** — VXLAN, auto peer discovery, encryption. Mostly legacy.

**Multus** — meta-plugin. Attach Pod to multiple networks (telco/NFV).

```yaml
annotations:
  k8s.v1.cni.cncf.io/networks: sriov-net, macvlan-net
```

## eBPF vs iptables

```mermaid
flowchart LR
    subgraph ipt["iptables (traditional)"]
        direction TB
        P1["Packet arrives"] --> R1["traverses chain of rules linearly"]
        R1 --> M1["each rule: match? apply action"]
        M1 --> S1["O(n) with number of rules<br>10,000 services = 10,000+ rules"]
    end

    subgraph eb["eBPF (Cilium, Calico eBPF)"]
        direction TB
        P2["Packet arrives"] --> R2["eBPF program at kernel hook"]
        R2 --> M2["hash map lookup: O(1)"]
        M2 --> S2["no userspace, no rule chains<br>10,000 services = same speed as 10"]
    end
```

eBPF: JIT-compiled, kernel-verified, kernel-space. At scale: big latency + CPU win.

## Cross-Node Packet Walk (Calico+BGP)

```mermaid
sequenceDiagram
    participant PodA as Pod A<br>10.244.1.5
    participant Node1 as Node 1<br>192.168.1.11
    participant PhysNet as Physical Network
    participant Node2 as Node 2<br>192.168.1.12
    participant PodB as Pod B<br>10.244.2.8

    PodA->>Node1: 1. Packet: src=10.244.1.5 dst=10.244.2.8
    Note over Node1: 2. veth pair → host namespace
    Note over Node1: 3. Route: 10.244.2.0/24 via 192.168.1.12 (BGP)
    Node1->>PhysNet: 4. Forwarded to Node 2
    PhysNet->>Node2: 4. Delivered to Node 2
    Note over Node2: 5. Route: 10.244.2.0/24 → local bridge (cni0)
    Note over Node2: bridge → correct veth
    Node2->>PodB: 6. Packet delivered, src=10.244.1.5
```

No NAT. Real Pod IPs end-to-end = flat network model.

## DNS

```mermaid
sequenceDiagram
    participant Pod
    participant Resolv as /etc/resolv.conf
    participant CoreDNS as CoreDNS<br>10.96.0.10
    participant API as API Server

    Pod->>Resolv: curl http://api-service
    Resolv->>CoreDNS: query: api-service.default.svc.cluster.local
    CoreDNS-->>Pod: 10.96.45.12 (Service ClusterIP)
    Note over CoreDNS,API: CoreDNS watches API server<br>for Service/Endpoint changes
```

CoreDNS watches API server for Service/Endpoint changes. Forwards external names.

Pod `/etc/resolv.conf`:

```
search default.svc.cluster.local svc.cluster.local cluster.local
nameserver 10.96.0.10
```

Bare `curl api-service` auto-expands to FQDN.

## Mental Models

| Concept | = |
|---|---|
| Network namespace | Pod's isolated network stack |
| veth pair | Patch cable Pod ↔ host |
| Linux bridge | Virtual switch on node |
| CNI plugin | Contractor wiring it up |
| VXLAN overlay | Tunnel through physical net |
| BGP | Teach routers where Pod subnets live |
| eBPF | Kernel programs, O(1) packet decisions |

← [[300 - Containers/Kubernetes/README|Kubernetes]] | [[Kubernetes MOC]]
