# AWS CloudFormation

Infrastructure as Code (IaC) service. Define AWS resources in a template — CloudFormation provisions and manages them.

---

## Core Concepts

**Template** — JSON or YAML file describing resources to create.

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-app-bucket
```

**Stack** — single unit of related resources deployed from one template. Create, update, or delete all resources together.

**Change Set** — preview of what will change before applying an update. Avoid surprises on production stacks.

---

## How It Works

1. Write template (YAML/JSON)
2. Upload to CloudFormation (directly or via S3)
3. CloudFormation creates a **stack** — provisions all defined resources in correct dependency order
4. Update template → CloudFormation diffs and updates only changed resources
5. Delete stack → all resources torn down together

---

## Key Benefits

| Benefit | What it means |
|---|---|
| Reproducibility | Same template = identical environment every time |
| Dependency ordering | CloudFormation resolves resource creation order automatically |
| Rollback | Failed stack update auto-rolls back to last stable state |
| Drift detection | Detects when actual resource state differs from template |
| Free | No charge for CloudFormation itself — pay only for resources created |

---

## Template Structure

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: Optional description

Parameters:       # Input values at deploy time
  EnvType:
    Type: String
    Default: dev

Resources:        # Required — the actual AWS resources
  MyEC2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t3.micro

Outputs:          # Values to export or display after deployment
  InstanceId:
    Value: !Ref MyEC2
```

---

## CloudFormation vs Other IaC Tools

| | CloudFormation | Terraform |
|---|---|---|
| Provider | AWS-native | Multi-cloud |
| Language | YAML / JSON | HCL |
| State file | Managed by AWS | Local / remote |
| Cost | Free | Free (OSS) |

---

## Related Services

- **AWS CDK** — write CloudFormation templates using real code (TypeScript, Python, etc.), compiles down to CloudFormation
- **AWS SAM** — CloudFormation extension for serverless (Lambda, API Gateway, DynamoDB)
- **Service Catalog** — distribute approved CloudFormation templates across an org

---

← [[AWS/Cloud Practitioner/Index]] · [[Home]]
