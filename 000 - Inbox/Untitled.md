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
