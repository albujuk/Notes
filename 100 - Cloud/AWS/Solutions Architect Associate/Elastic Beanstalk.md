---
domain: aws
track: solutions-architect-associate
topic: compute
type: note
tags:
  - aws
  - solutions-architect-associate
  - compute
  - elastic-beanstalk
  - paas
---

# Elastic Beanstalk

## The Developer Problem on AWS

Developers face the same set of concerns on every project:

- Managing infrastructure (EC2, ASG, ELB, RDS)
- Deploying code
- Configuring databases, load balancers, scaling
- Wanting consistency across environments (dev, test, prod)

Most web apps share the same architecture (ALB + ASG). All developers really want is for their code to run, consistently.

---

## Overview

Elastic Beanstalk is a **developer-centric view** of deploying applications on AWS. It is a managed service that orchestrates the components you already know ([[EC2]], [[100 - Cloud/AWS/Solutions Architect Associate/ELB|ELB]], ASG, [[RDS]]) so you can focus on code.

**What Beanstalk manages:**
- Capacity provisioning
- Load balancing
- Auto scaling
- Application health monitoring
- Instance configuration

**What you manage:**
- Application code
- Configuration of the environment (you retain full control)

**Pricing:** Beanstalk itself is free; you pay for the underlying resources (EC2 instances, etc.).

---

## Instantiating Applications Quickly

Launching a full stack (EC2, EBS, RDS) takes time: installing applications, inserting data, configuring everything. The cloud offers shortcuts.

### EC2 Instances

| Approach | How it works |
|----------|-------------|
| **Golden AMI** | Pre-install applications, OS dependencies, etc. Launch instances from this AMI |
| **User Data bootstrap** | Dynamic configuration via startup scripts (see [[EC2#Instance Bootstrapping|EC2 Bootstrapping]]) |
| **Hybrid** | Mix Golden AMI + User Data (this is what Elastic Beanstalk does) |

### RDS Databases

Restore from a **snapshot**: the database comes up with schemas and data already loaded.

### EBS Volumes

Restore from a **snapshot**: the disk is already formatted and has data.

---

## Components

| Component | Description |
|-----------|-------------|
| **Application** | Collection of Beanstalk components (environments, versions, configurations) |
| **Application Version** | An iteration of your application code |
| **Environment** | A collection of AWS resources running one application version (only one version at a time) |

### Environment Tiers

| Tier | Purpose |
|------|---------|
| **Web Server** | Serves HTTP/HTTPS requests; fronted by a load balancer |
| **Worker** | Processes background tasks from an SQS queue |

You can create multiple environments per application (dev, test, prod) and update application versions across them.

---

## Supported Platforms

- Go
- Java SE
- Java with Tomcat
- .NET Core on Linux
- .NET on Windows Server
- Node.js
- PHP
- Python
- Ruby
- Packer Builder
- Single Container Docker
- Multi-container Docker
- Preconfigured Docker

---

## Beanstalk at SAA Depth

Cloud Practitioner covers Beanstalk at awareness level ([[100 - Cloud/AWS/Cloud Practitioner/Compute#Elastic Beanstalk|Compute: Beanstalk]]). At SAA depth, know:

- **Deployment policies**: All at once, Rolling, Rolling with additional batch, Immutable, Traffic splitting (Blue/Green)
- **Configuration**: `.ebextensions` files for custom resource configuration
- **Health monitoring**: uses CloudWatch under the hood; environment health transitions (Green, Yellow, Red, Grey)
- **Scaling triggers**: CloudWatch alarms on CPU, network, request count, or custom metrics

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
