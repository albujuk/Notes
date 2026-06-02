Elastic Load Balancer ELB is a managed service by aws.

it's used to
1. spread load across multiple servers/instances
2. expose single point of access to the app 
3. handle failure of serrvers
4. ssl termination
5. health check servers behind it
6. enforce stickiness with cookies 
7. HA across zones
8. Separate public traffic and private

---
It will cost less than managing your own LB, and aws manages it so you have less things to worry about.
it also provides better integration with other AWS Services e.g. route 53, waf, global accelrator...

Some LB can be internal or external LBs
# AWS LB Types
1. Classic LB, v1 - Old Gen 2009 (deprecated): http/s, tcp, ssl.
2. Application Load Balancer (ALB) 2016: http/s, websockets
3. Network Load Balancer (NLB): TCP, UDP, TLS
4. Gateway Load Balancer (GWLB): at Layer 3, IP Protocol


