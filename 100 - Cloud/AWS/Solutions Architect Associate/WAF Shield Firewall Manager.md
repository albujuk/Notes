---
domain: aws
track: solutions-architect-associate
topic: security
type: note
tags:
  - aws
  - solutions-architect-associate
  - security
  - waf
  - shield
  - firewall-manager
  - ddos
---

# WAF, Shield, and Firewall Manager

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS WAF (Web Application Firewall)|Cloud Practitioner: WAF]], [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS Shield|Cloud Practitioner: Shield]].

## AWS WAF (Web Application Firewall)

Protects web applications from common web exploits at Layer 7 (HTTP).

### Deployment Targets

- Application Load Balancer
- API Gateway
- CloudFront
- AppSync GraphQL API
- Cognito User Pool

### Web ACL Rules

Define rules in a Web Access Control List (Web ACL):

| Rule Type | Description |
|-----------|-------------|
| **IP Set** | Up to 10,000 IP addresses; use multiple rules for more |
| **HTTP headers, body, URI strings** | Protect from SQL injection and Cross-Site Scripting (XSS) |
| **Size constraints** | Block requests based on size |
| **Geo-match** | Block traffic from specific countries |
| **Rate-based rules** | Count occurrences of events for DDoS protection |

**Key points:**
- Web ACLs are Regional except for CloudFront (global)
- A rule group is a reusable set of rules that you can add to a web ACL

### WAF with Load Balancer: Fixed IP

WAF does not support Network Load Balancer (Layer 4). For fixed IP with WAF on ALB, use Global Accelerator.

## AWS Shield: DDoS Protection

DDoS (Distributed Denial of Service): many requests at the same time.

### Shield Standard

- Free service activated for every AWS customer
- Protection from SYN/UDP Floods, Reflection attacks, and other Layer 3/Layer 4 attacks

### Shield Advanced

- Optional DDoS mitigation service (,000 per month per organization)
- Protects against sophisticated attacks on EC2, ELB, CloudFront, Global Accelerator, Route 53
- 24/7 access to AWS DDoS response team (DRP)
- Protects against higher fees during usage spikes due to DDoS
- Automatic application layer DDoS mitigation: automatically creates, evaluates, and deploys AWS WAF rules to mitigate Layer 7 attacks

## AWS Firewall Manager

Manage security rules across all accounts in an AWS Organization.

### Security Policy

Common set of security rules applied organization-wide:

| Rule Type | Resources Protected |
|-----------|---------------------|
| **WAF rules** | ALB, API Gateway, CloudFront |
| **AWS Shield Advanced** | ALB, CLB, NLB, Elastic IP, CloudFront |
| **Security Groups** | EC2, ALB, ENI resources in VPC |
| **AWS Network Firewall** | VPC-level protection |
| **Route 53 Resolver DNS Firewall** | DNS-level filtering |

**Key points:**
- Policies are created at the region level
- Rules are applied to new resources as they are created (good for compliance across all and future accounts)

## WAF, Shield, and Firewall Manager Together

These services work together for comprehensive DDoS protection:

| Scenario | Service Choice |
|----------|----------------|
| Define Web ACL rules | WAF |
| Granular protection of specific resources | WAF alone |
| Use WAF across accounts, automate protection of new resources | Firewall Manager with WAF |
| Frequent DDoS attacks, need dedicated support | Shield Advanced |

**Shield Advanced adds:**
- Dedicated support from Shield Response Team (SRT)
- Advanced reporting
- Automatic Layer 7 DDoS mitigation

## DDoS Best Practices

See [[100 - Cloud/AWS/Solutions Architect Associate/DDoS Best Practices|DDoS Best Practices]] for detailed mitigation techniques, infrastructure layer defense, EC2 instance selection, Auto Scaling, ELB configuration, and Shield Advanced benefits.

---

← [[100 - Cloud/AWS/Solutions Architect Associate/Security Services|Security Services]] · [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
