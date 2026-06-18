---
domain: aws
track: solutions-architect-associate
topic: messaging
type: note
tags:
  - aws
  - solutions-architect-associate
  - messaging
  - sqs
  - sns
  - kinesis
  - decoupling
---

# Messaging & Integration

When deploying multiple applications, they need to communicate. Two patterns:

1. **Synchronous:** App to App (direct calls)
2. **Asynchronous:** App to Queue to App

> [!warning] Problem with synchronous communication
> Synchronous communication breaks under traffic spikes (e.g., suddenly encoding 1000 videos instead of 10). Decouple applications using:
> - **SQS:** queue model
> - **SNS:** pub/sub model
> - **Kinesis:** real-time streaming model
> 
> These services scale independently from your application.

> For foundational messaging concepts, see [[100 - Cloud/AWS/Cloud Practitioner/Messaging|Cloud Practitioner: Messaging]].

---

## Amazon SQS: Standard Queue

Oldest AWS offering (over 10 years old). Fully managed service used to decouple applications.

| Attribute | Detail |
|-----------|--------|
| **Throughput** | Unlimited |
| **Messages in queue** | Unlimited |
| **Retention** | Default 4 days, max 14 days |
| **Latency** | <10 ms on publish and receive |
| **Message size limit** | 256 KB (1,024 KB per message sent) |
| **Delivery** | At least once (occasionally duplicate messages) |
| **Ordering** | Best effort (can have out-of-order messages) |

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]