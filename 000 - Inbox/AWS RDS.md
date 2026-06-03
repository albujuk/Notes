- Postgres
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server
- IBM DB2
- Aurora (AWS Proprietary database)

RDS is a managed service, it provides:
- Automated provisioning, OS patching
- Continuous backups and restore to specific timestamp (Point in Time Restore)!
- Monitoring dashboards
- Read replicas for improved read performance
- Multi AZ setup for DR (Disaster Recovery)
- Maintenance windows for upgrades
- Scaling capability (vertical and horizontal)
- Storage backed by EBS

**BUT you can’t SSH into your instances**

----

# Replicas VS Multi AZ
## Replicas
1. Up to 15 replicas
2. Replicas are async
3. replicas can be promoted to a real db
4. app must update connection string to use the replicas
**Use Case:** For read (`SELECT` Statements) only, it reduces the read load for the main DB
**Network Costs:** In aws there is a cost when transferring data from an az to another, this does not apply for the replicas, it's free, but it costs to move between regions.


## Multi AZ
1. Main use is for Disaster Recovery
2. SYNC replication
3. One DNS name – automatic app
4. failover to standby
5. Increase availability
6. Failover in case of loss of AZ, loss of network, instance or storage failure
7. No manual intervention in apps
8. Not used for scaling
The Read Replicas can be setup as Multi AZ for Disaster Recovery

