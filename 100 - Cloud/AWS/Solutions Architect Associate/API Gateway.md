---
domain: aws
track: solutions-architect-associate
topic: networking
type: note
tags:
  - aws
  - solutions-architect-associate
  - networking
  - api-gateway
  - endpoints
---

# API Gateway

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/Networking#API Gateway|Cloud Practitioner: API Gateway]].

## Endpoint Types

### Edge-Optimized (default)

For global clients.

- Requests are routed through the CloudFront Edge locations (improves latency)
- The API Gateway still lives in only one region

### Regional

For clients within the same region.

- Could manually combine with CloudFront (more control over the caching strategies and the distribution)

### Private

Can only be accessed from your VPC using an interface VPC endpoint (ENI).

- Use a resource policy to define access

## ACM Integration (Custom Domain Names)

Create a Custom Domain Name in API Gateway to use TLS certificates.

### Edge-Optimized (default)

For global clients.

- Requests are routed through the CloudFront Edge locations (improves latency)
- The API Gateway still lives in only one region
- **The TLS Certificate must be in the same region as CloudFront, in us-east-1**
- Then setup CNAME or (better) A-Alias record in Route 53

### Regional

For clients within the same region.

- **The TLS Certificate must be imported on API Gateway, in the same region as the API Stage**
- Then setup CNAME or (better) A-Alias record in Route 53

---

← [[100 - Cloud/AWS/Solutions Architect Associate/Security Services|Security Services]] · [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
