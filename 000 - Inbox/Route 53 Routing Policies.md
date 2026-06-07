Don't confuse it with load balancer routing

#   Simple
- Typically, route traffic to a single resource
- Can specify multiple values in the same record
- If multiple values are returned, a random one is chosen by the **client**
- When Alias enabled, specify only one AWS resource
- Can’t be associated with Health Checks
