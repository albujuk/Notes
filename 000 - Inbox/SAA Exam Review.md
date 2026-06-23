1. VPC sharing can share subnets, not the actual VPC
2. route 53 is good for wight routing traffic for green/blue deployments but it's not the best option when there is dns caching, use global accelerator 
3. Amazon S3 Batch Replication provides you a way to replicate objects that existed before a replication configuration was in place, objects that have previously been replicated, and objects that have failed replication. This is done through the use of a Batch Operations job.


redshift supports MPP

When the new AMI is copied from Region A into Region B, it automatically creates a snapshot in Region B because AMIs are based on the underlying snapshots. Further, an instance is created from this AMI in Region B. Hence, we have 1 Amazon EC2 instance, 1 AMI and 1 snapshot in Region B.


