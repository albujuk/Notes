---
domain: aws
track: solutions-architect-associate
topic: security
type: note
tags:
  - aws
  - solutions-architect-associate
  - security
  - ddos
  - best-practices
---

# DDoS Best Practices

> Builds on [[100 - Cloud/AWS/Solutions Architect Associate/WAF Shield Firewall Manager|WAF, Shield, Firewall Manager]].

## Infrastructure Layer Defense

- **CloudFront**: Web application delivery at edge, DDoS protection
- **Global Accelerator**: Fixed IP, Shield integration, for backends not compatible with CloudFront
- **Route 53**: Domain resolution at edge, DDoS protection
- **EC2 with Auto Scaling**: Scale during traffic surges (flash crowds or DDoS)
- **Elastic Load Balancing**: Scales with traffic, distributes to multiple EC2 instances

## Mitigation Techniques (AWS Edge Services)

AWS services that operate from edge locations (CloudFront, Global Accelerator, Route 53) provide comprehensive availability protection against all known infrastructure layer attacks.

**Benefits:**
- Access to internet and DDoS mitigation capacity across the AWS Global Edge Network (terabit-scale attacks)
- AWS Shield DDoS mitigation integrated with edge services: sub-second time-to-mitigate
- Stateless SYN Flood mitigation using SYN cookies before passing to protected service
- Automatic traffic engineering to disperse or isolate large volumetric attacks
- Application layer defense for CloudFront when combined with WAF

**No charge for inbound data transfer. You do not pay for DDoS attack traffic mitigated by AWS Shield.**

## EC2 Instance Selection for DDoS Resilience

Use EC2 instance types with enhanced networking for high traffic volumes:
- Up to 100 Gbps network bandwidth interfaces
- Enhanced networking: higher I/O performance, higher bandwidth, lower CPU utilization
- Recommended: Dedicated Instances or instances with "N" suffix (e.g., `c6gn.16xlarge`, `c5n.18xlarge`, `c5n.metal`)

## EC2 with Auto Scaling (BP7)

Operate at scale to mitigate infrastructure and application layer attacks:
- Use load balancers to distribute traffic to overprovisioned or auto-scaling EC2 instances
- Set CloudWatch alarms to initiate Auto Scaling based on CPU, RAM, Network I/O, or custom metrics
- Protects availability during unexpected request volume increases
- TLS negotiation handled by CloudFront or load balancer, protecting instances from TLS-based attacks
- Scale horizontally (add instances) or vertically (larger instance types)
- **RDS Proxy**: Pool and share database connections to handle unpredictable surges
- **RDS storage auto-scaling**: Automatically increase database storage capacity

## Elastic Load Balancing (BP6)

**Application Load Balancer:**
- Routes traffic based on content, accepts only well-formed web requests
- Blocks many common DDoS attacks (SYN floods, UDP reflection)
- Automatically scales to absorb additional traffic during attacks
- Scaling activities due to infrastructure layer attacks are transparent and do not affect your bill

**Network Load Balancer:**
- For non-HTTP/HTTPS applications at ultra-low latency
- TCP SYN or UDP traffic reaching NLB on valid listener is routed to targets (not absorbed), except TLS-listeners which terminate TCP connection
- **Recommendation:** Deploy Global Accelerator to protect against SYN flood for TCP listeners
- Use Shield Advanced to configure DDoS protection for Elastic IP addresses assigned to NLB

**Security Group Connection Tracking:**
- To avoid exhausting connection tracking limits during DDoS attacks, configure security groups for untracked connections:
  - Inbound rule: accept TCP from any IP (0.0.0.0/0 or ::/0)
  - Outbound rule: allow response traffic for all ports (0-65535) to any IP
- This allows CLB and ALB to scale based on traffic increases without connection tracking limits
- Only helps when DDoS traffic originates from sources allowed by security group

## Detect and Filter Malicious Requests

- **CloudFront**: Cache static content, serve from edge, protect backend
- **WAF on CloudFront and ALB**: Filter and block requests based on signatures
- **WAF rate-based rules**: Automatically block IPs of bad actors
- **WAF managed rules**: Block attacks based on IP reputation, block anonymous IPs
- **CloudFront geo restriction**: Block specific geographies
- **Shield Advanced**: Automatic Layer 7 DDoS mitigation with WAF rules

## Obfuscating AWS Resources

- Use CloudFront, API Gateway, ELB to hide backend resources (Lambda, EC2)
- Security groups and NACLs to filter traffic by IP at subnet or ENI level
- Elastic IPs protected by Shield Advanced

## Protecting API Endpoints

- Hide EC2, Lambda, and other backend resources
- Edge-optimized mode, or CloudFront + regional mode (more DDoS control)
- WAF + API Gateway: burst limits, headers filtering, API keys

## Shield Advanced Benefits

- 24/7 access to AWS Shield Response Team (SRT)
- Proactive engagement feature for DDoS mitigation
- Sensitive detection thresholds for faster mitigation
- Tailored Layer 7 detection based on baselined traffic patterns
- Automatic application layer DDoS mitigation (creates, evaluates, deploys WAF rules)
- AWS WAF at no additional cost for DDoS mitigation (with CloudFront or ALB)
- AWS Firewall Manager at no additional cost
- Cost protection: request refund of scaling-related costs from DDoS attacks
- Enhanced SLA
- Protection groups: bundle resources for customized detection and mitigation scope
- DDoS attack visibility via Console, API, CloudWatch metrics and alarms

---

← [[100 - Cloud/AWS/Solutions Architect Associate/WAF Shield Firewall Manager|WAF, Shield, Firewall Manager]] · [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
