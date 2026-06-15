- Content Delivery Network (CDN)
- Improves read performance, content is cached at the edge
- Improves users experience
- Hundreds of Points of Presence globally (edge locations, caches)
- DDoS protection (because worldwide), integration with Shield, AWS Web Application Firewall

CloudFront Origins
- S3 bucket
	- For distributing files and caching them at the edge
	- For uploading files to S3 through CloudFront
	- Secured using Origin Access Control (OAC)
- VPC Origin
	- For applications hosted in VPC private subnets
	- Private Application Load Balancer / Network Load Balancer / EC2 Instances
- Custom Origin (HTTP)
	- S3 website (must first enable the bucket as a static S3 website)
	- Any public HTTP backend you want (example: Public ALB)

CloudFront vs S3 Cross Region Replication
- CloudFront:
- Global Edge network
- Files are cached for a TTL (maybe a day)
- Great for static content that must be available everywhere
- S3 Cross Region Replication:
- Must be setup for each region you want replication to happen
- Files are updated in near real-time
- Read only
- Great for dynamic content that needs to be available at low-latency in few regions

CloudFront – ALB or EC2 as an origin Using VPC Origins
- Allows you to deliver content from your applications hosted in your VPC private subnets (no need to expose them on the Internet)
- Deliver traffic to private:
	- Application Load Balancer
	- Network Load Balancer
	- EC2 Instances

The resource behind cloud front must be public 
so 