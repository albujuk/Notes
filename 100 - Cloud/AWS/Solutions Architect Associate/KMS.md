---
domain: aws
track: solutions-architect-associate
topic: security
type: note
tags:
  - aws
  - solutions-architect-associate
  - security
  - encryption
  - kms
  - key-management
  - multi-region
  - client-side-encryption
---

# KMS (Key Management Service)

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS KMS (Key Management Service)|Cloud Practitioner: KMS]].

Anytime you hear encryption for an AWS service, it's most likely KMS. AWS manages encryption keys for us, fully integrated with IAM for authorization. Able to audit KMS Key usage using CloudTrail. Seamlessly integrated into most AWS services (EBS, S3, RDS, SSM).

**Never store secrets in plaintext, especially in code.** KMS Key Encryption is available through API calls (SDK, CLI). Encrypted secrets can be stored in code or environment variables.

## KMS Key Types

### Symmetric Keys (AES-256)

- Single encryption key used to Encrypt and Decrypt
- AWS services that are integrated with KMS use Symmetric CMKs
- You never get access to the KMS Key unencrypted (must call KMS API to use)

### Asymmetric Keys (RSA & ECC key pairs)

- Public (Encrypt) and Private Key (Decrypt) pair
- Used for Encrypt/Decrypt, or Sign/Verify operations
- The public key is downloadable, but you can't access the Private Key unencrypted
- Use case: encryption outside of AWS by users who can't call the KMS API

## KMS Key Types (Ownership)

| Type | Cost | Example |
|------|------|---------|
| **AWS Owned Keys** | Free | SSE-S3, SSE-SQS, SSE-DDB (default key) |
| **AWS Managed Key** | Free | aws/service-name (example: aws/rds or aws/ebs) |
| **Customer managed keys created in KMS** | $1 / month | Custom keys you create |
| **Customer managed keys imported** | $1 / month | Keys you import from external source |

+ pay for API call to KMS ($0.03 / 10000 calls)

## Automatic Key Rotation

| Key Type | Rotation |
|----------|----------|
| **AWS-managed KMS Key** | Automatic every 1 year |
| **Customer-managed KMS Key** | Automatic & on-demand (must be enabled) |
| **Imported KMS Key** | Only manual rotation possible using alias |

## KMS Key Policies

Control access to KMS keys, similar to S3 bucket policies. **Difference:** you cannot control access without them.

### Default KMS Key Policy

- Created if you don't provide a specific KMS Key Policy
- Complete access to the key to the root user = entire AWS account

### Custom KMS Key Policy

- Define users, roles that can access the KMS key
- Define who can administer the key
- Useful for cross-account access of your KMS key

## Copying Snapshots Across Accounts

1. Create a Snapshot, encrypted with your own KMS Key (Customer Managed Key)
2. Attach a KMS Key Policy to authorize cross-account access
3. Share the encrypted snapshot
4. (in target) Create a copy of the Snapshot, encrypt it with a CMK in your account
5. Create a volume from the snapshot

## KMS Multi-Region Keys

Identical KMS keys in different AWS Regions that can be used interchangeably.

**Key characteristics:**
- Multi-Region keys have the same key ID, key material, automatic rotation
- Encrypt in one Region and decrypt in other Regions
- No need to re-encrypt or making cross-Region API calls
- KMS Multi-Region are NOT global (Primary + Replicas)
- Each Multi-Region key is managed independently

**Use cases:**
- Global client-side encryption
- Encryption on Global DynamoDB
- Global Aurora

## Global DynamoDB and KMS Multi-Region Keys Client-Side Encryption

We can encrypt specific attributes client-side in our DynamoDB table using the Amazon DynamoDB Encryption Client.

Combined with Global Tables, the client-side encrypted data is replicated to other regions. If we use a multi-region key, replicated in the same region as the DynamoDB Global table, then clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side.

Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to an API key.

## Global Aurora and KMS Multi-Region Keys Client-Side Encryption

We can encrypt specific attributes client-side in our Aurora table using the AWS Encryption SDK.

Combined with Aurora Global Tables, the client-side encrypted data is replicated to other regions. If we use a multi-region key, replicated in the same region as the Global Aurora DB, then clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side.

Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to an API key, we can protect specific fields even from database admins.

## S3 Replication Encryption Considerations

| Encryption Type | Replication Behavior |
|-----------------|----------------------|
| **Unencrypted objects** | Replicated by default |
| **SSE-S3** | Replicated by default |
| **SSE-C** (customer provided key) | Can be replicated |
| **SSE-KMS** | You need to enable the option |

For SSE-KMS replication:
- Specify which KMS Key to encrypt the objects within the target bucket
- Adapt the KMS Key Policy for the target key
- An IAM Role with kms:Decrypt for the source KMS Key and kms:Encrypt for the target KMS Key
- You might get KMS throttling errors, in which case you can ask for a Service Quotas increase
- You can use multi-region AWS KMS Keys, but they are currently treated as independent keys by Amazon S3 (the object will still be decrypted and then encrypted)

## KMS vs CloudHSM

> Builds on [[100 - Cloud/AWS/Cloud Practitioner/Security#AWS CloudHSM|Cloud Practitioner: CloudHSM]].

| Feature | AWS KMS | AWS CloudHSM |
|---------|---------|--------------|
| **Tenancy** | Multi-Tenant | Single-Tenant |
| **Standard** | FIPS 140-2 Level 3 | FIPS 140-2 Level 3 |
| **Master Keys** | AWS Owned CMK, AWS Managed CMK, Customer Managed CMK | Customer Managed CMK |
| **Key Types** | Symmetric, Asymmetric, Digital Signing | Symmetric, Asymmetric, Digital Signing & Hashing |
| **Key Accessibility** | Accessible in multiple AWS regions (can't access keys outside the region it's created in) | Deployed and managed in a VPC, can be shared across VPCs (VPC Peering) |
| **Cryptographic Acceleration** | None | SSL/TLS Acceleration, Oracle TDE Acceleration |
| **Access & Authentication** | AWS IAM | You create users and manage their permissions |
| **High Availability** | AWS Managed Service | Add multiple HSMs over different AZs |
| **Audit Capability** | CloudTrail, CloudWatch | CloudTrail, CloudWatch, MFA support |
| **Free Tier** | Yes | No |

**CloudHSM details:**
- KMS: AWS manages the software for encryption
- CloudHSM: AWS provisions encryption hardware (dedicated HSM = Hardware Security Module)
- You manage your own encryption keys entirely (not AWS)
- HSM device is tamper resistant, FIPS 140-2 Level 3 compliance
- Supports both symmetric and asymmetric encryption (SSL/TLS keys)
- No free tier available
- Must use the CloudHSM Client Software
- Redshift supports CloudHSM for database encryption and key management
- Good option to use with SSE-C encryption

**High Availability:**
- CloudHSM clusters are spread across Multi AZ (HA)
- Great for availability and durability

**IAM permissions:**
- CRUD an HSM Cluster

**CloudHSM Software:**
- Manage the Keys
- Manage the Users

**AWS manages the hardware**

---

← [[100 - Cloud/AWS/Solutions Architect Associate/Security Services|Security Services]] · [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]