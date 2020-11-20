# EC2 Storage: EBS & EFS

## Elastic Block Storage (EBS)

- An EBS Volume is a network drive you can attach to your instances while they run
- Persists data
- Locked to an AZ
- Have a provisioned capacity, and billed for all
- 4 types:
  - GP2 (SSD)
    - general purpose SSD that balances price and performance
    - for most workloads
    - 1GiB - 16TiB
    - Small GP2 can burst IOPS to 3000
    - 3 IOPS per GB
    - Min IOPS 100
    - Max IOPS 16000, will be reached at 5334 GB
  - IO1 (SSD)
    - highest performance, for mission critical low-latency or high-through put
    - for critical business apps that require sustained IOPS performance, or more than 16000 IOPS per volume
    - large database
    - 4G - 16T
    - IOPS <- [100, 32000 (64000 for Nitro)]
    - Max IOPS/size = 50:1
  - ST1 (HDD)
    - low cost for frequently accessed, throughput intensive
    - Big data, data warehouses, log, kafka
    - 500G - 16T
  - SC1 (HDD)
    - lowest cost for less frequently accessed
    - 500G - 16T
    - Max IOPS 250
- Only GP2 and IO1 are for root volumes
- Snapshots
  - incremental
  - consumes IO
  - stored in S3
  - can copy across AZ or region
  - can make AMI from snapshot
  - EBS volumes restored by snapshots need to be pre-warmed
  - Can manage by snapshot lifecycle manager
- Migration
  - snapshot -> copy -> create volume from snapshot
- Encryption
  - to encrypt an unencrypted EBS: create snapshot -> encrypt snapshot using copy -> create new EBS volume from snapshot

- EBS vs Instance Store
  - only certain types can have instance store
  - instance store is physically attached to the machine, no network
  - better I/O
  - good for buffer / cache / scratch data / temporary content
  - data survives reboots, but will be lost upon stop
  - cannot resize

- EBS RAID
  - RAID 0: increase IOPS, for performance, not fault tolerant (add up size, add up IOPS)
  - RAID 1: redundancy, for backup (same size, same IOPS)

## Elastic Files System (EFS)

- Managed NFS that can be mounted on many EC2
- Works across AZ
- Highly available, scalable, expensive, pay per use
- content management, web serving, data sharing, Wordpress
- NFSv4.1 protocol
- Compatible only with Linux
- Has general purpose / max io performance modes
- Storage tier (lifecycle management - move file after N days): standard / infrequent access (EFS-IA)

