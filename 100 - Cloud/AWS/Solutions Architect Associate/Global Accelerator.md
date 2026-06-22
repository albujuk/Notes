---
domain: aws
track: solutions-architect-associate
topic: networking
type: note
tags:
  - aws
  - solutions-architect-associate
  - networking
  - global-accelerator
  - anycast
---

# AWS Global Accelerator

Leverages the AWS internal network to route traffic to your applications. Uses **Anycast IPs** to send traffic to the nearest edge location, then routes over the AWS backbone to your application.

See [[100 - Cloud/AWS/Cloud Practitioner/Networking#Global Accelerator|Networking: Global Accelerator]] for the high-level overview.

## Unicast vs Anycast IP

| | Unicast | Anycast |
|---|---|---|
| **IP ownership** | One server holds one IP | All servers hold the same IP |
| **Routing** | Traffic goes to that specific server | Client routed to the nearest server |
| **Use case** | Traditional | Global load distribution |

Global Accelerator creates **2 Anycast IPs** for your application. Traffic enters at edge locations and is routed over the AWS network to your endpoints.

## Features

- Works with **Elastic IP, EC2 instances, ALB, NLB, public or private**
- **Consistent performance**: intelligent routing to lowest latency, fast regional failover, no client cache issues (IP doesn't change), uses internal AWS network
- **Health checks**: global health checks on your applications, failover in less than 1 minute for unhealthy endpoints, great for disaster recovery
- **Security**: only 2 external IPs to whitelist, DDoS protection via [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Shield|Shield]]

## Global Accelerator vs [[100 - Cloud/AWS/Solutions Architect Associate/CloudFront|CloudFront]]

Both use the AWS global network and edge locations. Both integrate with Shield for DDoS protection.

| | CloudFront | Global Accelerator |
|---|---|---|
| **Content** | Cacheable + dynamic content at the edge | Non-cacheable TCP/UDP traffic |
| **Protocol** | HTTP/HTTPS, WebSockets | TCP, UDP (any protocol) |
| **Edge behavior** | Content served at edge | Packets proxied at edge to origin |
| **Static IP** | No (DNS-based) | Yes (2 Anycast IPs) |
| **Failover** | DNS-based (TTL dependent) | Sub-minute (health checks + static IP) |
| **Best for** | Web content, images, videos, API acceleration | Gaming (UDP), IoT (MQTT), VoIP, HTTP with static IP needs, deterministic regional failover |

**Decision rule**: need caching? → CloudFront. Need static IP + any TCP/UDP? → Global Accelerator.

### Why Global Accelerator doesn't help with traffic spikes

Global Accelerator does **not** cache content. Every request still hits the origin (ALB/EC2). It optimizes the network path (faster routing, health-check-based failover), but the origin servers must handle the full request volume. Against a traffic spike, GA provides no relief — the backend takes the same load. [[100 - Cloud/AWS/Solutions Architect Associate/CloudFront|CloudFront]] absorbs spikes by serving cached responses from edge locations, only forwarding cache misses to the origin.

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]] · [[100 - Cloud/AWS/Solutions Architect Associate/CloudFront|CloudFront]] · [[100 - Cloud/AWS/Solutions Architect Associate/ELB|ELB]]