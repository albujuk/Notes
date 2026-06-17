---
domain: aws
track: solutions-architect-associate
topic: database
type: note
tags:
  - aws
  - solutions-architect-associate
  - database
  - elasticache
  - redis
  - memcached
  - caching
---

# Amazon ElastiCache

Fully managed in-memory caching service. Compatible with Redis, Valkey, and Memcached.

> For foundational ElastiCache concepts (latency, use cases), see [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon ElastiCache|Cloud Practitioner: ElastiCache]].

## Redis vs Memcached

| Feature             | Redis                                                                   | Memcached                                                      |
| ------------------- | ----------------------------------------------------------------------- | -------------------------------------------------------------- |
| **Data structures** | Rich (strings, hashes, lists, sets, sorted sets, bitmaps, hyperloglogs) | Simple (strings only)                                          |
| **Persistence**     | Yes (RDB snapshots, AOF logs)                                           | No                                                             |
| **Replication**     | Multi-AZ with automatic failover                                        | None                                                           |
| **Clustering**      | Yes (sharding across nodes)                                             | No (manual sharding only)                                      |
| **Pub/Sub**         | Yes                                                                     | No                                                             |
| **Lua scripting**   | Yes                                                                     | No                                                             |
| **Transactions**    | Yes (MULTI/EXEC)                                                        | No                                                             |
| **Use case**        | Complex data, session state, leaderboards, real-time analytics          | Simple key-value caching, high-throughput read-heavy workloads |

## Caching Strategies

### Lazy Loading (Cache-Aside)

- App reads from cache first
- On cache miss, app reads from database and writes result to cache
- Data in cache can become stale if database changes without cache update
- Good for read-heavy workloads where data changes infrequently

### Write-Through

- App writes to cache and database simultaneously
- Cache is always up-to-date
- Higher write latency (two writes per operation)
- Good for workloads where data consistency is critical

### Session Store

- Store user session data in ElastiCache instead of application servers
- Enables stateless application servers
- Session data survives instance termination
- Common pattern with [[100 - Cloud/AWS/Solutions Architect Associate/ELB|ELB]] and [[100 - Cloud/AWS/Solutions Architect Associate/EC2#Auto Scaling Groups (ASG)|Auto Scaling Groups]]

## Redis Clustering

### Cluster Mode Enabled

- Data partitioned across multiple shards
- Each shard has a primary and optional replicas
- Scales horizontally for large datasets and high throughput
- Single configuration endpoint for client connections

### Cluster Mode Disabled

- Single shard with primary and replicas
- Simpler setup, lower operational overhead
- Limited to single-node capacity
- Good for smaller datasets or when you need Multi-AZ without sharding complexity

## High Availability

### Multi-AZ with Automatic Failover (Redis)

- Primary node in one AZ, replica(s) in different AZ(s)
- Automatic failover promotes replica to primary when primary fails
- Failover typically completes in seconds
- No manual intervention required

### Read Replicas (Redis)

- Up to 5 read replicas per primary
- Offload read traffic from primary
- Can be promoted to standalone clusters
- Asynchronous replication

## Integration Patterns

### Database Query Caching

- Cache results of expensive database queries
- Reduces database load and improves response time
- Set TTL based on data freshness requirements
- Common with [[100 - Cloud/AWS/Solutions Architect Associate/RDS|RDS]] and [[100 - Cloud/AWS/Cloud Practitioner/Database#Amazon DynamoDB|DynamoDB]]

### API Response Caching

- Cache API responses in ElastiCache
- Reduces backend load
- Improves API response time
- Can be combined with [[100 - Cloud/AWS/Cloud Practitioner/Networking#API Gateway|API Gateway]] caching

---

← [[100 - Cloud/AWS/Solutions Architect Associate/README|Solutions Architect Associate]]
