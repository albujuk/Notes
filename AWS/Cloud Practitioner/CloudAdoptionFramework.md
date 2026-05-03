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
| **Migrate & Modernize** | AWS DMS | Migrates homogeneous and heterogeneous databases; low downtime; supports ongoing replication; handles TB-scale at low cost |
| **Migrate & Modernize** | AWS SCT | Converts schema and code objects (stored procedures, views, functions) between database engines; used before DMS for heterogeneous migrations |
| **Migrate & Modernize** | AWS DataSync | Automates and accelerates large data transfers between on-premises storage and S3/EFS; handles encryption, bandwidth throttling, scheduling, and task reporting |
| **Migrate & Modernize** | AWS Transfer Family | Fully managed file transfers into/out of S3 and EFS over SFTP, FTPS, FTP; managed encryption and authentication |
| **Migrate & Modernize** | AWS Direct Connect | Dedicated private connection to AWS; fast, reliable, high-bandwidth data transfer during migration. See [[Connectivity#AWS Direct Connect\|Direct Connect]] |
| **Migrate & Modernize** | Snowball Edge Storage Optimized | Physical device for offline migration; NVMe storage; petabyte-scale; used when bandwidth is limited, no internet, or data volume makes online transfer impractical |

---

← [[README]] · [[Home]]
