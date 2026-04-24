---
domain: aws
track: cloud-practitioner
topic: monitoring
type: note
tags:
  - aws
  - cloud-practitioner
  - monitoring
  - cloudwatch
  - cloudtrail
  - trusted-advisor
  - aws-health
---

# AWS Monitoring & Observability

Services for tracking resource performance, auditing activity, and surfacing operational health across your AWS environment.

---

## Amazon CloudWatch

Monitors AWS resources and applications in real time. Provides system-wide visibility into resource utilization, application performance, and operational health.

**Use cases:** Set alarms on metrics, collect and analyze logs, trigger automated actions in response to changes.

---

## AWS CloudTrail

Records user activity and API calls across AWS services as events. Enables auditing, security monitoring, and operational troubleshooting.

> Answers: **Who did what, where, and when?**

**Use cases:** Audit access, investigate incidents, track configuration changes across accounts.

---

## AWS Trusted Advisor

Continuously evaluates your AWS environment using best practice checks across five categories:

- Cost optimization
- Performance
- Security
- Fault tolerance
- Service limits

Recommends actions to remediate deviations from best practices.

---

## AWS Health

Data source for events and changes affecting your AWS Cloud resources. Notifies you about:

- **Service events** — outages or degradations in AWS services
- **Planned changes** — scheduled maintenance or deprecations
- **Account notifications** — actions required on your account

**Use cases:** Proactively manage impact of AWS events on your resources.

---

← [[Index]] · [[Home]]