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
	1. Up to 3GiB/s for reads and 1GiB/s for writes
	2.  Used for unpredictable workloads
