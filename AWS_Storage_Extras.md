# AWS Storage Extras

## Snowball

- Physical data transport solution
- alternative to moving data over the network
- secure, tamper resistant, uses KMS 256 encryption
- tracking using SNS and text messages, E-ink shipping label
- pay per job
- use cases: data cloud migrations, DC decommission, disaster recovery
- if it takes more than a week to transfer over the network, use Snowball
- Snowball Edge
  - add computational capacity
  - supports a custom EC2 AMI so can perform processing on the go
  - supports custom lambda functions
  - useful to pre-process the data while moving
  - use case: data migration, image collation, IoT capture, machine learning
- Snowmobile
  - transfer exabytes of data
  - each has 100 PB of capacity
  - better than snowball if more than 10 PB
- cannot import to Glacier directly

## Storage Gateway

- expose S3 data on-premise
- use cases: disaster recovery, backup and restore, tiered storage
- three types:
  - File Gateway
    - configured S3 buckets are accessible using NFS and SMB protocol
    - bucket access using IAM roles for each file gateway
    - most recently used data is cached
    - can be mounted on many servers
    - File access / NFS
  - Volume Gateway
    - block storage using iSCSI protocol
    - backed by EBS snapshots which can help restore on-premise volumes
    - **cached volumes**: low latency access to most recent data
    - **stored volumes**: entire dataset is on premise, scheduled backups to S3
    - Volumes / Block storage / iSCSI
  - Tape Gateway
    - Virtual Tape Library (VTL)
    - backup data using existing tape-based processes and iSCSI interface
    - works with leading backup software vendors
    - VTL Tape solution / backup with iSCSI
- File gateway hardware appliance: helpful for daily NFS backups in small data centers with no capability of virtualization

## Amazon FSx

- FSx for windows
  - fully managed windows file system share drive
  - supports SMB protocol and windows NTFS
  - supports AD integration
  - can be accessed from on-premise
  - can be configured to be multi-AZ
  - data backed up daily to S3
- FSx for Lustre
  - Lustre is a type of parallel distributed file system, for large-scale computing
  - lustre = linux + cluster
  - for machine learning, high performance computing, video processing, financial modeling, electronic design automation...
  - seamless integrate with S3
  - can be used from on-premise servers

