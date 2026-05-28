### IAM Policies

These are **JSON documents** that define _what actions are allowed or denied_ on _which resources_. Think of them as the rulebook.

### IAM Roles
A role is an **identity without permanent credentials** it's assumed temporarily by a user, service, or application. 
For example, an EC2 instance assumes a role to access S3, rather than having hardcoded credentials.
Roles have policies attached to them that define what the role can do.

By default, AWS IAM role session credentials are valid for 1 hour, but the maximum session duration can be configured to up to 12 hours (43,200 seconds).
### Access Keys

These are **long-term credentials** (Access Key ID + Secret Access Key) used to authenticate programmatically, via CLI, SDK, or API calls. 
They prove _who you are_, not _what you can do_.

- Permissions are still determined by the **policies** attached to that user
- They're associated with **IAM users**, not roles
- Considered less secure than roles because they're static and can leak

### The Mental Model

|Concept|Answers the question...|Example|
|---|---|---|
|**Policy**|What am I _allowed_ to do?|Allow `s3:PutObject` on `my-bucket`|
|**Role**|_Who/what_ am I (temporarily)?|EC2 instance acting as a backup service|
|**Access Key**|How do I _prove_ who I am?|CLI credentials for a CI/CD pipeline|