# Databases in AWS

## Database Types

- **RDBMS (SQL / OLTP)**: RDS, Aurora - great for joins
- **NoSQL**: DynamoDB (~=JSON), ElastiCache (kv pairs), Neptune (graphs) - no joins, no SQL
- **Object Store**: S3, Glacier
- **Data Warehouse (SQL Analytics / BI)**: Redshift (OLAP), Athena
- **Search**: ElasticSearch (JSON) - free text, unstructured searches
- **Graphs**: Neptune - displays relationships between data

## RDS

- managed PostgreSQL / MySQL/  Oracle / SQL Server
- must provision an EC2 instance & EBS volume type and size
- support for read replicas and multi a
- security through IAM, security groups, KMS, SSL in transit
- Backup / snapshot / point in time restore feature
- managed and scheduled maintenance
- monitoring through CloudWatch
- Use Case: store relational datasets (RDBMS / OLTP), perform SQL queries, transactional inserts / update / delete is available

## Aurora

- Compatible API for PostgreSQL / MySQL
- Data is held in 6 replicas, across 3 AZ
- Auto healing capability
- multi AZ, auto scaling read replicas
- read replicas can be global
- Aurora database can be Global for DR or latency purposes
- auto scaling of storage from 10GB to 64TB
- define EC2 instance type for aurora instances
- same security / monitoring / maintenance features as RDS
- Aurora Serverless option
- Use Case: same as RDS, but with less maintenance / more flexibility / more performance

## ElastiCache

- managed Redis / Memcached
- in-memory data store, sub-millisecond latency
- must provision an ec2 instance type
- support or clustering (Redis) and multi AZ, read replicas (sharding)
- security through IAM, Security Groups, KMS, Redis Auth
- Backup / snapshot / point in time restore feature
- managed and scheduled maintenance
- monitoring through CloudWatch
- Use case: K/V store, frequent reads, less writes, cache results for DB queries, store session data for websites, cannot use SQL

## DynamoDB

- AWS proprietary technology, managed NoSQL database
- serverless, provisioned capacity, auto scaling, on demand capacity
- can replace ElastiCache as a K/V store
- highly available, multi AZ by default, read and writes are decoupled, DAX for read cache
- reads can be eventually consistent or strongly consistent
- security, authentication and authorization is done through IAM
- DynamoDB Streams to integrate with AWS Lambda
- backup / restore feature, Global table feature
- monitoring through CloudWatch
- can only query on primary key, sort key, or indexes
- Use case: serverless apps development (small documents 100s KB), distributed serverless cache, doesn't have SQL query language available, has transactions capability

## S3

- a K/V store for objects
- great for big objects
- serverless, scales infinitely, max object size is 5TB
- eventually consistency for overwrites and deletes
- tiers: S3 standard, S3 IA, S3 One Zone IA, Glacier for backups
- features: versioning, encryption, cross region replication, etc
- security: IAM, bucket policies, ACL
- encryption: SSE-S3, SSE-KMS, SSE-C, client side encryption, SSL in transit
- Use case: static files, key value store for big files, website hosting

## Athena

- fully serverless database with SQL capabilities
- used to query data in S3
- pay per query
- output results back to S3
- secured through IAM
- Use case: one time SQL queries, serverless queries on S3, log analytics

## Redshift

- Redshift is based on PostgreSQL, but is not used for OLTP
- OLAP - online analytical processing (analytics and data warehousing)
- 10x better performance than other data ware houses, scale to PBs of data
- Columnar storage of data
- massively parallel query execution (MPP), highly available
- pay as you go based on the instances provisioned
- has a SQL interface for performing the queries
- BI tools such as AWS Quicksight or Tableau integrate with it
- data is loaded from S3, BynamoDB, DMS, orther DBs
- from 1 to 128 nodes, up to 160 GB of space per node
- leader note: for query planning, results aggregation
- compute note: for performing the queries, send results to leader
- Redshift Spectrum: perform queries directly against S3
- backup & restore, security VPC / IAM / KMS, monitoring
- Redshift enhanced VPC routing: copy / unload goes through VPC
- can configure Amazon Redshift to automatically copy snapshots (automated or manual) of a cluster to another AWS region
- can query data that is already in S3 without loading it
- must have a Redshift cluster available to start the query
- the query is then submitted to thousands of Redshift Spectrum nodes

## Neptune

- fully manged graph database
- for high relationship data, social networking, knowledge graphs...
- highly available across 3 AZ, with up to 15 read replicas
- point-in-time recovery, continuous backup to S3
- support for KMS encryption at rest + HTTPS

## ElasticSearch

- search any field, even partially matches
- as a complement to another database
- has some usage for Big Data apps
- can provision a cluster of instances
- built-in integrations: Amazon Kinesis Data Firehose, AWS IoT, and Amazon CloudWatch Logs for data ingestion
- security through Cognito & IAM, KMS encryption, SSL & VPC
- comes with Kibana (visualization) & Logstach (log ingestion) - ELK stack

