VPC Peering
• Privately connect two VPCs using AWS’
network
• Make them behave as if they were in the
same network
• Must not have overlapping CIDRs
• VPC Peering connection is NOT transitive
(must be established for each VPC that
need to communicate with one another)
• You must update route tables in each VPC’s
subnets to ensure EC2 instances can
communicate with each other
VPC Peering – Good to know
• You can create VPC Peering connection between VPCs in different AWS
accounts/regions
• You can reference a security group in a peered VPC (works cross
accounts – same region)

VPC Endpoints (AWS PrivateLink)
• Every AWS service is publicly exposed
(public URL)
• VPC Endpoints (powered by AWS
PrivateLink) allows you to connect to AWS
services using a private network instead of
using the public Internet
• They’re redundant and scale horizontally
• They remove the need of IGW, NATGW, …
to access AWS Services
• In case of issues:
• Check DNS Setting Resolution in your VPC
• Check Route Tables

Types of Endpoints
• Interface Endpoints (powered by PrivateLink)
	• Provisions an ENI (private IP address) as an entry point (must attach a Security Group)
	• Supports most AWS services
	• $ per hour + $ per GB of data processed
• Gateway Endpoints
	• Provisions a gateway and must be used as a
	target in a route table (does not use security groups)
	• Supports both S3 and DynamoDB
	• Free

• Gateway is most likely going to be
 preferred all the time at the exam
 • Cost: free for Gateway, $ for
 interface endpoint
 • Interface Endpoint is preferred
 access is required from on-
 premises (Site to Site VPN or
 Direct Connect), a different VPC
 or a different region
