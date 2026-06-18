# Integration & Messaging

## Application Communication Patterns

When deploying multiple applications, they need to communicate. Two patterns:

1. **Synchronous:** App to App
2. **Asynchronous:** App to Queue to App

Synchronous communication can be problematic under traffic spikes (e.g., suddenly encoding 1000 videos instead of 10). Better to decouple applications using:

- **SQS:** queue model
- **SNS:** pub/sub model
- **Kinesis:** real-time streaming model

These services scale independently from your application.

---

## Amazon SQS: Standard Queue

- Oldest offering (over 10 years old)
- Fully managed service, used to decouple applications

**Attributes:**

| Attribute | Detail |
|-----------|--------|
| Throughput | Unlimited |
| Messages in queue | Unlimited |
| Retention | Default 4 days, max 14 days |
| Latency | <10 ms on publish and receive |
| Message size limit | 256 KB (1,024 KB per message sent) |
| Delivery | At least once (occasionally duplicate messages) |
| Ordering | Best effort (can have out-of-order messages) |
