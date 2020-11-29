# Decoupling Applications: SQS, SNS, Kinesis, Active MQ

## SQS

- Standard queue
  - decouple apps
  - unlimited throughput, unlimited number of messages in queue
  - default retention of 4 days, max 14 days
  - low latency
  - max 256 per message
  - consumers delete messages after processing
  - can have duplicate messages (at lease once delivery)
  - can have out of order messages (best effort ordering)
  - IAM policies and SQS Access Policies (useful for allowing other services or cross account access)
  - Message Visibility Timeout
    - after a message is polled by a consumer, it becomes invisible to other consumers
    - default 30 seconds
    - if message not processed within the visibility timeout, it will be process twice
    - call ChangeMessageVisiblity API to change the timeout
- Dead letter queues
  - MaximumReceives set to put a message to DLQ
  - good to set a high retention period
- Delay queues
  - delay a message up to 15 minutes
  - default 0
  - override using DelaySeconds parameter
- FIFO queues
  - first in first out
  - limited throughput
  - exactly-once send

## SNS

- producer sends message to one SNS topic
- many receivers listen to the SNS topic notifications
- up to 10 million subscriptions per topic
- 100,000 topics
- subscribers can be:
  - SQS
  - HTTP/HTTPS
  - Lambda
  - Emails
  - SMS messages
  - Mobile Notifications
- publishers can be:
  - CloudWatch (for alarms)
  - ASG notifications
  - S3 (on bucket events)
  - CloudFormation (upon state changes => failed to build, etc)
  - etc
- Security similar to SQS
- SNS + SQS fan out pattern
  - push once in SMS, receive in all SQS queues that are subscribers
  - make sure SQS queue access policy allows for SNS to write
  - SNS cannot send messages to SQS FIFO queues

## Kinesis

- a managed alternative to Apache Kafka
- good for app logs, metric, IoT, clickstreams
- good for real-time big data
- good for streaming processing frameworks
- data auto replicated to 3 AZs
- Kinesis Streams
  - low latency streaming ingest at scale
  - stream divided in ordered shards
  - higher throughput => add number of shards
  - data retention: default 1day, can go up to 7 days
  - ability to process / replay data
  - multiple apps can consume the same stream
  - real-time processing with scale of throughput
  - once data inserted, can't be deleted (immutability)
  - Shards
    - one stream is made of many shards
    - 1Mb/s or 1000 messages at write per shard
    - 2Mb/s at read per shard
    - billing per shard
    - records are ordered per shard
  - same key goes to the same partition
  - choose a partition key that is highly distributed
  - PrivisionedThroughputExceeded if go over the limits (retries with backoff, increase shards, ensure partition key is good)
- Kinesis Data Analytics
  - perform real-time analytics on streams using SQL
  - managed
  - pay for actual consumption rate
  - can create streams out of the real-time queries
- Kinesis Firehose
  - load streams into S3, Redshift, ElasticSearch and Splunk
  - fully managed
  - near real time
  - pay for amount of data
  - no data storage

## Amazon MQ

- Traditional apps running from on-premise may use open protocols such as: MQTT, AMQP, STOMP, Openwire, WSS
- Amazon MQ = managed Apache ActiveMQ
- doesn't scale as much as SQS / SNS
- runs on a dedicated machine
- has both queue feature and topic feature

