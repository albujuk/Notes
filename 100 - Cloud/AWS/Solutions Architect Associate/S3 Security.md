---
domain: aws
track: solutions-architect-associate
topic: storage
type: note
tags:
  - aws
  - solutions-architect-associate
  - storage
  - s3
  - encryption
  - security
  - cors
  - mfa-delete
  - presigned-urls
  - object-lock
  - access-points
---

# S3 Security

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/S3#Security|Cloud Practitioner: S3 Security]] (bucket policies, ACLs, block public access overview).

## Object Encryption

Four encryption methods for S3 objects:

| Method | Key Management | Use Case |
|--------|----------------|----------|
| **SSE-S3** | AWS-managed keys | Default for new buckets/objects |
| **SSE-KMS** | AWS KMS | User control + CloudTrail audit. Subject to KMS rate limits (5500-30000 req/s by region) |
| **SSE-C** | Customer-provided | Keys never stored by AWS. HTTPS required |
| **Client-Side** | Customer fully manages | Encrypt before upload, decrypt after download using S3 Client-Side Encryption Library |

### HTTP Headers

| Method | Header | Notes |
|--------|--------|-------|
| SSE-S3 | `x-amz-server-side-encryption: AES256` | Applied automatically |
| SSE-KMS | `x-amz-server-side-encryption: aws:kms` | Can specify KMS key ID with `x-amz-server-side-encryption-aws-kms-key-id` |
| SSE-C | `x-amz-server-side-encryption-customer-algorithm`, `x-amz-server-side-encryption-customer-key`, `x-amz-server-side-encryption-customer-key-MD5` | Key sent in headers per request |

**SSE-KMS APIs:** `GenerateDataKey` (upload), `Decrypt` (download). Request quota increase via Service Quotas Console if needed.

## CORS (Cross-Origin Resource Sharing)

**Origin** = scheme + host + port (e.g. `https://www.example.com`).

When a client makes cross-origin requests to your S3 bucket, enable CORS headers (`Access-Control-Allow-Origin`). Can allow specific origins or `*` (all). Popular exam question.

## MFA Delete

Requires MFA code to:
- Permanently delete an object version
- Suspend versioning on the bucket

Does **not** require MFA to:
- Enable versioning
- List deleted versions

**Requirements:** versioning must be enabled. Only bucket owner (root) can enable/disable MFA Delete.

## Pre-Signed URLs

Generate via S3 Console, CLI, or SDK. URL inherits permissions of the generating user for GET/PUT.

**Expiration:**
- Console: 1 min to 720 mins (12 hours)
- CLI: `--expires-in` parameter (default 3600s, max 604800s ~ 7 days)

**Use cases:** temporary access for logged-in users, dynamic file sharing, time-limited uploads.

## Glacier Vault Lock

WORM (Write Once Read Many) model. Create vault lock policy, then lock it (cannot be changed or deleted). For compliance and data retention.

## Object Lock

Requires **versioning enabled**. WORM model that blocks object version deletion for a specified time.

| Mode | Behavior |
|------|----------|
| **Compliance** | No user (including root) can overwrite/delete or change retention. Retention periods cannot be shortened |
| **Governance** | Most users blocked; special permissions can alter retention or delete |

**Retention Period:** protect object for fixed duration (can be extended).

**Legal Hold:** protect indefinitely, independent of retention. Placed/removed using `s3:PutObjectLegalHold` permission.

## Access Points

Simplify security management at scale. Each access point has:
- Own DNS name (Internet or VPC origin)
- Access point policy (similar to bucket policy)

Can restrict access to VPC-only via VPC Endpoint (Gateway or Interface). VPC Endpoint Policy must allow access to target bucket and access point.

## Object Lambda

Use Lambda functions to transform objects before retrieval. Create S3 Access Point + S3 Object Lambda Access Point on top of one bucket.

**Use cases:** redact PII for analytics, convert XML to JSON, resize/watermark images on-the-fly based on caller.

---

← [[100 - Cloud/AWS/Solutions Architect Associate/S3|S3]] · [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
