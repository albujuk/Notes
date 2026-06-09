Don't confuse it with load balancer routing

#   Simple
- Typically, route traffic to a single resource
- Can specify multiple values in the same record
- If multiple values are returned, a random one is chosen by the **client**
- When Alias enabled, specify only one AWS resource
- Can’t be associated with Health Checks

# Weighted
- Control the % of the requests that go to each specific resource
- Assign each record a relative weight
- DNS records must have the same name and type
- Can be associated with Health Checks
- Use cases: load balancing between regions, testing new application version
- Assign a weight of 0 to a record to stop sending traffic to a resource
- If all records have weight of 0, then all records will be returned equally

# Latency
- Redirect to the resource that has the least latency close to users
- Super helpful when latency for users is a priority
- Latency is based on traffic between users and AWS Regions
- Can be associated with Health Checks (has a failover capability)

# Health Checks
