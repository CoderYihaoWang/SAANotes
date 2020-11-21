# AWS Fundamentals: RDS, Aurora, ElastiCache

## Relational Database Service (RDS)

- Postgres, MySQL, MariaDB, Oracle, MS SQLServer
- managed service
- automated provisioning, OS patching
- continuous backups and restore to specific timestamp
  - daily full backup
  - transaction logs backed-up every 5 minutes
  - 7 days retention
- monitoring dashboards
- scaling capability
- storage backed by EBS
- cannot SSH into
- Read replicas
  - Up to 5 read replicas
  - within AZ, cross AZ or cross region (network cost when cross AZ or regions)
  - Async replication -> eventually consistent
  - replicas can be promoted to their own DB
  - apps must update the connection string to leverage read replicas
  - Can be set up as multi AZ for Disaster Recovery
- Multi AZ for disaster recovery
  - Sync replication
  - One DNS name, auto failover, no manual intervention
  - not used for scaling
- Security
  - Encryption
    - At rest: with KMS (AES-256 encryption), has to be defined at the launch time. If master not encrypted, replicas cannot be encrypted. **Transparent Data Encryption (TDE)** available for Oracle and SQL Server
    - In flight: SSL certificates. Parameter groups (PostgreSQL) or run command (MySQL) to enforce.
  - Network and IAM
    - usually in private subnet
    - using security groups
    - IAM policies help control who can manage AWS RDS
    - traditional username and password used for logging into the database
    - IAM-base authentication available for MySQL and PostgreSQL; using authentication token obtained through IAM and RDS API calls. 15 min lifetime

## Aurora

- proprietary for AWS, compatible with MySQL or PostgreSQL, high performance, 20% more expensive
- automatically grows in increments of 10 Gb, up to 64 TB
- instantaneous failover
- 6 copies across 3 AZ, self-healing
- storage striped across 100s of volumes
- 1 master for write, 15 read replicas. any can be master when the master fails
- support cross region replication
- one writer endpoint, one read endpoint, load balanced
- security similar to EDS, IAM authentication available
- Aurora serverless
  - automated database instantiation and auto-scaling based on actual usage
  - good for infrequent, intermittent or unpredictable workloads
  - no capacity planning
  - pay per second
- Aurora Global Database
  - 1 primary region
  - up to 5 secondary read only regions, replication lag less than 1 second
  - up to 16 read replicas per secondary regions
  - helps for decreasing latency
  - promoting another region less than 1 min

## ElastiCache

- managed Redis or Memcached
- in memory databased with really high performance, low latency
- helps make app stateless (writing session data to ElastiCache)
- write scaling using sharding
- read scaling using read replicas
- multi AZ with failover capability
- Redis
  - multi AZ with auto-failover
  - read replicas to scale reads and have high availability
  - data durability using AOF persistence
  - backup and restore features
- Memcached
  - multi-node for partitioning of data (sharding)
  - non persistence
  - no backup and restore
  - multi threaded architecture

- Security
  - support SSL
  - do not support IAM authentication
  - IAM policies only for API-level security
  - Redis AUTH: can set password/token
  - Memcached: supports ASAL-base authentication
- Patterns
  - Lazy loading: all the read data is cached, data can become stale
  - Write through: adds or update data in the cache when written to a DB, no stale data
  - Session store: store temporary session data in a cache, using TTL features