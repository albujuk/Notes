---
domain: aws
track: cloud-practitioner
topic: messaging
type: note
tags:
  - aws
  - cloud-practitioner
  - messaging
  - sqs
  - sns
  - eventbridge
  - event-driven
  - pub-sub
---

# Messaging & Event-Driven Services

Services for communication between application components. Together they support event-driven, decoupled architectures that scale reliably.

---

## Amazon EventBridge

Serverless event bus. Routes events from sources (custom apps, AWS services, third-party SaaS) to target services — without tight coupling between producers and consumers.

- You define **rules**: filter events by pattern, then route to a target ([[Compute#AWS Lambda|Lambda]], [[#Amazon SQS (Simple Queue Service)|SQS]], [[#Amazon SNS (Simple Notification Service)|SNS]], etc.)
- Handles receive → filter → transform → deliver
- If a target is unavailable, EventBridge retries — events aren't lost
- Replaces the older CloudWatch Events service (same underlying tech, superset of features)

**Use when:** many services need to react to the same events, or you need cross-account / cross-service routing.

---

## Amazon SQS (Simple Queue Service)

Message queue. Decouples producers from consumers — the sender drops a message in the queue; the receiver pulls and processes it on its own schedule.

- Messages persist in the queue until a consumer retrieves and deletes them
- Scales automatically; handles any message volume
- **Standard queue**: at-least-once delivery, best-effort ordering
- **FIFO queue**: exactly-once processing, strict order

**Use when:** you need to buffer work between components, or tolerate consumers being temporarily unavailable.

---

## Amazon SNS (Simple Notification Service)

Publish-subscribe service. A publisher sends one message to an **SNS topic**; SNS fans it out to all subscribers simultaneously.

- Subscribers: [[Compute#AWS Lambda|Lambda]], [[#Amazon SQS (Simple Queue Service)|SQS]], HTTP/S endpoints, email, SMS, mobile push
- No polling — push-based delivery
- One message → many destinations in one shot

**Use when:** one event must trigger multiple independent consumers at once (e.g. send email + trigger Lambda + write to SQS).

---

## Comparison

| | EventBridge | SQS | SNS |
|---|---|---|---|
| **Model** | Event bus (routing rules) | Queue (pull) | Pub/sub (push) |
| **Consumers** | 1 target per rule (fan-out via multiple rules) | 1 consumer per message | Many subscribers per topic |
| **Delivery** | Push | Pull | Push |
| **Best for** | Event routing, cross-service orchestration | Decoupling, buffering work | Fan-out to multiple endpoints |

> SQS + SNS combo: SNS fans out to multiple SQS queues — each queue gets its own copy of the message for independent processing.

---

← [[Index]] · [[Home]]
