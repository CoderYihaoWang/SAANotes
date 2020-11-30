# Serverless Overviews from a Solution Architect Perspective

## Lambda

- virtual functions limited by time, running on demand
- pay per request and compute time
- easy monitoring through CloudWatch
- easy to get more resources per function
- increasing RAM will also improve CPU and network
- Docker does not run with Lambda, use ECS  / Fargate
- Limitations:
  - memory: 128 - 3008 MB, 64MB increments
  - max execution time: 15min
  - env vars: 4KB
  - disk capacity in the function containter (/tmp): 512MB
  - concurrent: 1000
  - deploy size (.zip) 50MB
  - uncompressed: 250 MB
  - can use /tmp directory to load other files at startup
- Lambda@Edge
  - deploy Lambda functions alongside CloudFront CDN
  - for:
    - website security and privacy
    - dynamic web app at edge
    - SEO
    - intelligently route across origins and data centers
    - bot mitigation at the edge
    - real-time image transformation
    - A/B testing
    - user authentication and authorization
    - user prioritization
    - user tracking an analytics

## DynamoDB

- fully managed, highly available with replication across 3 AZ
- NoSQL
- made of tables, each table has a primary key, infinite number of items
- Read Capacity Units (RCU): throughput for reads
  - 1 RCU= 1 strongly consistent read of 4KB per second
  - or 2 eventually consistent read of 4 KB per second
- Write Capacity Units (WCU): throughput for writes
  - 1 WCU = 1 write of 1KB per second
- DynamoDB Accelerator (DAX)
  - cache
  - writes go through DAX to DynamoDB
  - solves hot key problem
  - 5min ttl by default
- DynamoDB Streams
  - changes in DynamoDB can end up in a DynamoDB Stream
  - react to changes in real time 
  - analytics
  - create derivative tables
  - insert into ElasticSearch
  - need to enable streams to implement cross region replication
  - 24 hours of data retention

## API Gateway

- implement REST api end points
- supports WebSocket
- handle api versioning
- handle different environments
- handle security
- create api keys
- swagger
- transform and validate requests and responses
- generate SDK and API specifications
- cache api responses
- Endpoint types:
  - Edge-optimized (default): for global clients
  - Regional
  - Private
- Security:
  - IAM permissions
    - create an IAM policy authorization and attach to user / role
    - API Gateway verifies IAM permissions passed by the calling app
    - good to provide access within your own infrastructure
    - leverages Sig v4 where IAM credentials are in headers
  - Lambda authorizer
    - uses was lambda wot validate the token
    - option to cache
    - helps integrate with third party authentication
    - lambda must return an IAM policy for the user
  - Cognito user pools
    - Cognito fully manages user lifecycle
    - API gateway verifies identity automatically from AWS Cognito
    - no custom implementation required
    - only helps with authentication, not authorization
    - pools can be backed by Facebook, Google login, etc

## Cognito

- Cognito user pools (CUP)
  - username (email) / password combination
  - possible MFA
  - can enable federated identities
  - sends back a JWT
  - sign in functionality for ap users
  - integrate with API Gateway
- Cognito Identity Pools (Federated Identity)
  - provide AWS credentials to users so they can access AWS resources directly
  - integrate with Cognito user pools as an identity provider
- Cognito Sync
  - synchronize data from device to Cognito
  - may be replaced by AppSync

