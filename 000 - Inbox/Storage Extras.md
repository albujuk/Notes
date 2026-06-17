# AWS Snowball
- Highly-secure, portable devices to collect and process data at the edge, and migrate data into and out of AWS
- Helps migrate up to Petabytes of data

Use Snowball for the following Challenges:
- Limited connectivity
- Limited bandwidth
- High network cost
- Shared bandwidth (can’t maximize the line)
- Connection stability

AWS Snowball is offline devices to perform data migrations
If it takes more than a week to transfer over the network, use Snowball devices!

## Edge Computing
- Process data while it’s being created on an edge location
- A truck on the road, a ship on the sea, or whatever
- These locations may have limited internet and no access to computing power
- We setup a Snowball Edge device to do edge computing
	- Snowball Edge Compute Optimized (dedicated for that use case) & Storage Optimized
	- Run EC2 Instances or Lambda functions at the edge
- Use cases: preprocess data, machine learning, transcoding media
- Snowball **cannot** import to Glacier directly
- You must use Amazon S3 first, in combination with an **S3 lifecycle policy**

# Amazon FSx
- Launch 3rd party high-performance file systems on AWS
- Fully managed service
	1. FSx for LustreFSx for
	2. NetApp ONTAP
	3. FSx for Windows
	4. File ServerFSx for OpenZFS

## FSx for Windows
• FSx for Windows is a fully managed Windows file system share drive
• Supports **SMB protocol** & **Windows NTFS**
• Microsoft Active Directory integration, ACLs, user quotas
• Can be mounted on Linux EC2 instances
• Supports Microsoft's Distributed File System (DFS) Namespaces (group files across multiple FS)
• Scale up to 10s of GB/s, millions of IOPS, 100s PB of data
• Storage Options:
• SSD – latency sensitive workloads (databases, media processing, data analytics, …)
• HDD – broad spectrum of workloads (home directory, CMS, …)
• Can be accessed from your on-premises infrastructure (VPN or Direct Connect)
• Can be configured to be Multi-AZ (high availability)
• Data is backed-up daily to S3
