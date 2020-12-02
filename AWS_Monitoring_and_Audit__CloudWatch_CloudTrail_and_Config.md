# AWS Monitoring & Audit: CloudWatch, CloudTrail & Config

## CloudWatch

- Metrics
  - a variable to monitor
  - belong to namespaces
  - dimension is an attribute of a metric
  - up to 10 dimensions per metric 
  - have timestamps
  - can create CloudWatch dashboards of metrics
  - EC2 instance metrics
    - EC2 instance metrics have metrics every 5 minutes
    - with detailed monitoring, get data every 1 minute
    - EC2 memory usage is by default not pushed
  - Custom metrics
    - possible to define and send own custom metrics to CloudWatch
    - can use dimensions
    - resolution:
      - standard: 1 minute
      - high resolution: up to 1 second (StorageResolution API parameter)
    - use API call PutMetricData
    - use exponential back off in case of throttle errors

- Dashboards
  - are global
  - can include graphs from different regions
  - can change the time zone & time range of the dashboards
  - can setup automatic refresh (10s, 1m, 2m, 5m, 15m)
- Logs
  - apps can send logs to CloudWatch using the SDK
  - Log groups contain log streams
  - can define log expiration policies
  - make sure IAM permissions are correct
  - can use filter expressions
  - CloudWatch Logs Insights can be used to query logs and add queries to CloudWatch Dashboards
- Agents
  - by default, no logs from EC2 machine will go to CloudWatch
  - need to run a CloudWatch agent on EC2 to push the log files wanted
  - make sure IAM permissions are correct
  - the CloudWatch log agent can be setup on-premises
- Alarms
  - alarms are used to trigger notifications for any metric
  - can go to auto scaling, ec2 actions, SNS notifications
  - various options (sampling, %, max, min, etc)
  - alarm states: ok, insufficient_data, alarm
  - period
    - high resolution custom metrics: can only choose 10 sec or 30 sec
- Events
  - schedule: Cron jobs
  - event pattern: event rules to react to a service doing something
  - triggers to lambda functions, SQS/SNS/Kinesis messages
  - CloudWatch event creates a small JSON document to give information about the change

## CloudTrail

- provides governance, compliance and audit for AWS account
- enabled by default
- get an history of events / api calls made within AWS account by:
  - console
  - SDK
  - CLI
  - AWS services
- can put logs from CloudTrail into CloudWatch Logs
- look into CloudTrial first if a resource is deleted in AWS

## Config

- helps with auditing and recording compliance of AWS resources
- helps record configurations and changes over time
- possibility of storing the configuration data into S3
- can receive alerts (SNS notifications) for any changes
- per-region service 
- can be aggregated across regions and accounts