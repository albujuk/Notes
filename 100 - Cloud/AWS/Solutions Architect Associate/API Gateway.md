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

### ACM Certificate Region Requirements

| API Gateway Endpoint Type | Where ACM cert must live |
|---|---|
| **Edge-Optimized** | `us-east-1` (because of CloudFront) |
| **Regional** | Same region as the API Gateway (e.g., `us-west-2`) |
| **Private** | N/A — uses ACM Private CA / internal cert handling, not public ACM in the same way |

### Edge-Optimized (default)

For global clients.

- Requests are routed through the CloudFront Edge locations (improves latency)
- The API Gateway still lives in only one region
- **The TLS Certificate must be in us-east-1 (N. Virginia)** regardless of where the API Gateway itself is deployed
- This is because Edge-Optimized API Gateway is fronted by a CloudFront distribution that AWS manages on your behalf
- ACM certificates used by CloudFront must be requested or imported in `us-east-1`, no matter which region the underlying service lives in
- Then setup CNAME or (better) A-Alias record in Route 53

### Regional

For clients within the same region.

- **The TLS Certificate must be imported on API Gateway, in the same region as the API Stage**
- Then setup CNAME or (better) A-Alias record in Route 53

### Important Notes

- If you ever switch an API Gateway from Edge-Optimized to Regional, you need to re-issue/re-associate the certificate in the API Gateway's region instead — the `us-east-1` cert won't work for a regional custom domain name

---

← [[100 - Cloud/AWS/Solutions Architect Associate/Security Services|Security Services]] · [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
