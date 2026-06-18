---
domain: aws
track: solutions-architect-associate
topic: compute
type: note
tags:
  - aws
  - solutions-architect-associate
  - compute
  - lambda
  - serverless
  - concurrency
---

# AWS Lambda

> For foundational Lambda concepts, see [[100 - Cloud/AWS/Cloud Practitioner/Compute#AWS Lambda|Cloud Practitioner: Lambda]].

---

## Concurrency

In Lambda, **concurrency** is the number of in-flight requests that your function is currently handling.

### Reserved Concurrency

Sets both the **maximum and minimum** number of concurrent instances allocated to your function.

**Key characteristics:**
- When a function has reserved concurrency, **no other function can use that concurrency**
- Useful for ensuring critical functions always have enough concurrency to handle incoming requests
- Can be used for **limiting concurrency** to prevent overwhelming downstream resources (like database connections)
- Acts as both a **lower and upper bound**: reserves the specified capacity exclusively while preventing scaling beyond that limit
- **No additional charges** for configuring reserved concurrency

### Provisioned Concurrency

The number of **pre-initialized execution environments** allocated to your function.

**Key characteristics:**
- Execution environments are **ready to respond immediately** to incoming function requests
- Useful for **reducing cold start latencies** for functions
- Designed to make functions available with **double-digit millisecond response times**
- **Best for interactive workloads:** applications with users initiating requests (web and mobile applications) that are most sensitive to latency
- **Asynchronous workloads** (data processing pipelines) are often less latency sensitive and usually don't need provisioned concurrency
- **Incurs additional charges** to your AWS account

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
