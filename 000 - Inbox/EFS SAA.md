EFS is managed network file system NFS, it can be global and mounted across az and regions

Highly available, scalable, expensive (3x gp2), pay per use (no prepay or provision)

Use cases: content management, web serving, data sharing, Wordpress
Uses NFSv4.1 protocol
Uses security group to control access to EFS
Compatible with Linux based AMI (not Windows)
Encryption at rest using KMS
POSIX file system (~Linux) that has a standard file API
File system scales automatically, pay-per-use, no capacity planning

## Scale
1000s of concurrent NFS clients/connections, 10 GB+ /s throughput
can scale and grow as usage increases without problems or planing needed

## Performance mode (At EFS Creation)
1. General Purpose Mode (Default): latency-sensitive use cases
2. Max I/O: higher latency, throughput, highly parallel (used with big data)

## Throughput Mode
1. Bursting – 1 TB = 50MiB/s + burst of up to 100MiB/s
2.  Provisioned – set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage
3. Elastic – automatically scales throughput up or down based on your workloads
	1.  Used for unpredictable workloads



## Storage Classes
Standard: For frequently accessed data and files 
Infrequent Access: cost-optimized for data that is accessed only a few times each quarter
Archive: cost-optimized for data that is accessed only a few times each year or less

### By Default:
- files that are not accessed in Standard storage class or 30 days are transitioned into IA.
- files that are not accessed in the Standard storage class for 90 days are transitioned in to the Archive storage class.
- files are not moved back to the Standard storage class, and they remain in the IA or Archive storage class when they are accessed.
    
    For performance-sensitive use cases that demand the fastest latency performance (such as applications that work with a large volume of small files), choose to transition files into Standard storage **On first access**.

Use multi az efs for prod, but for dev and test use one zone and it's compatible with IA

using the correct settings and classes will reduce the cost up to 90%

