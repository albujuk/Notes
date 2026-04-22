---
domain: aws
track: cloud-practitioner
topic: connectivity
type: note
tags:
  - aws
  - cloud-practitioner
  - connectivity
  - vpn
  - direct-connect
  - privatelink
  - transit-gateway
  - hybrid-cloud
---

# Connectivity — Connecting to AWS

## AWS Client VPN

Remote workers connect to AWS or on-premises resources over an encrypted VPN tunnel from their device. Client-based, OpenVPN-compatible.

## AWS Site-to-Site VPN

Links an entire on-premises network to a VPC over an encrypted IPsec tunnel through the internet. Uses a [[Networking#Virtual Private Gateway|Virtual Private Gateway]] on the AWS side and a customer gateway on the on-premises side.

## AWS PrivateLink

Exposes services privately inside AWS without traffic touching the public internet. Uses VPC endpoints — traffic stays on the AWS network. Common for accessing AWS services (S3, DynamoDB) or sharing your own service with other VPCs/accounts.

- **Interface endpoint** — elastic network interface with private IP; used for most AWS services and custom services
- **Gateway endpoint** — route table entry; only for S3 and DynamoDB (free)
- Traffic never leaves AWS backbone — no NAT, no internet gateway needed

## AWS Direct Connect

Dedicated physical network connection from on-premises to AWS. Bypasses the public internet entirely — lower latency, more consistent throughput. Takes weeks to provision; higher cost than VPN.

- Speeds: 1 Gbps, 10 Gbps, 100 Gbps (or sub-1G via hosted connection through a partner)
- Traffic is **not encrypted by default** — add Site-to-Site VPN on top for encryption
- Use case: large data transfers, latency-sensitive workloads, regulatory requirements

## AWS Transit Gateway

Hub-and-spoke transit hub that interconnects multiple VPCs and on-premises networks through a single gateway. Replaces complex VPC peering meshes.

- Attach VPCs, Site-to-Site VPNs, and Direct Connect connections to one gateway
- Route tables on the gateway control which attachments can talk to each other
- Supports inter-region peering (connect Transit Gateways across regions)
- Scales to thousands of VPCs

---

## VPN vs PrivateLink vs Direct Connect

|                  | Site-to-Site VPN                             | PrivateLink                            | Direct Connect                                     |
| ---------------- | -------------------------------------------- | -------------------------------------- | -------------------------------------------------- |
| **Traffic path** | Public internet (encrypted)                  | AWS backbone only                      | Dedicated private line                             |
| **Encryption**   | Yes (IPsec)                                  | Not needed (private)                   | No (add VPN on top)                                |
| **Connects**     | On-prem ↔ VPC                                | Service-to-service (within/across AWS) | On-prem ↔ AWS                                      |
| **Setup time**   | Minutes                                      | Minutes                                | Weeks                                              |
| **Cost**         | Low                                          | Per-endpoint + data                    | High (port hours + data)                           |
| **Bandwidth**    | Variable (internet-dependent)                | High (AWS network)                     | Consistent (up to 100 Gbps)                        |
| **Use case**     | Quick on-prem link, backup to Direct Connect | Private access to AWS/custom services  | Production workloads needing consistent throughput |

**Key distinctions:**
- VPN = encrypted tunnel *over* internet → cheap, fast to set up, bandwidth varies
- PrivateLink = traffic never leaves AWS → for service exposure, not on-prem connectivity
- Direct Connect = physical line → predictable performance, high cost, long lead time; often paired with VPN as encrypted backup

| Option | Path | Use case |
|--------|------|----------|
| Client VPN | Internet (encrypted) | Remote workers |
| Site-to-Site VPN | Internet (encrypted) | On-prem ↔ VPC |
| PrivateLink | AWS network | Private service access |
| Direct Connect | Dedicated line | High-throughput / low-latency on-prem link |

---

← [[Networking]] · [[Index]] · [[Home]]
