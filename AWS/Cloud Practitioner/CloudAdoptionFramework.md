---
domain: aws
track: cloud-practitioner
topic: cloud-adoption-framework
type: note
tags:
  - aws
  - cloud-practitioner
  - migration
  - caf
  - cloud-adoption
---

# AWS Cloud Adoption Framework (CAF)

Brings AWS best practices to companies migrating to the cloud. Accelerates migration, organizes resources, and aligns management.

---

## Six Perspectives

| Perspective | Focus | Roles |
|-------------|-------|-------|
| **Business** | Align IT with business goals; build cloud adoption business case | Business/finance managers, budget owners, strategy stakeholders |
| **People** | Organization-wide change management; identify skill/role gaps | HR, staffing, people managers |
| **Governance** | Align IT strategy with business strategy; minimize risk | CIO, program managers, enterprise architects, business analysts |
| **Platform** | Implement cloud technology for adoption requirements | — |
| **Security** | Meet visibility, auditability, control, and agility objectives | CISO, IT security managers/analysts |
| **Operations** | Enable and recover IT workloads per agreed service levels | IT operations/support managers |

---

## 7 Rs of Migration

| Strategy | Also called | Description |
|----------|-------------|-------------|
| **Relocate** | — | Move VMs/containers already running on-premises to cloud as-is |
| **Rehost** | Lift and shift | Move apps as-is; servers become VMs. Up to 30% cost savings |
| **Replatform** | Lift, tinker, and shift | Minor cloud optimizations; no core code changes (e.g. MySQL → RDS) |
| **Refactor** | Rearchitect | Redesign using cloud-native features; highest cost, most impact |
| **Repurchase** | Drop and shop | Replace app with cloud-based version (e.g. from AWS Marketplace) |
| **Retain** | Stays where it lays | Keep near-EOL apps; only migrate what makes sense |
| **Retire** | — | Remove unused apps; 10%+ of portfolios qualify |

---

## Migration Services

| Phase | Service | Purpose |
|-------|---------|---------|
| **Assess** | Migration Evaluator | Data-driven business case; analyzes current state, projects cloud costs, surfaces licensing reuse |
| **Mobilize** | Application Discovery Service | Discovers on-premises server inventory, connections, and dependencies |
| **Mobilize** | Migration Hub | Centralized hub for full migration lifecycle — planning, execution, tracking; free |
| **Migrate & Modernize** | Application Migration Service | Lifts and modernizes on-premises/cloud apps; supports any OS; no downtime during replication |

---

← [[Index]] · [[Home]]
