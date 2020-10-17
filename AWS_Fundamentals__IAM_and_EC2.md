# AWS Fundamentals: IAM & EC2

## AWS Regions and AZs

- **Regions**: clusters of data centers
- Most AWS services are region-scoped
- **AZ (Availability Zones)**: Each region has many AZs.
- Each AZ is one or more discrete data centers with redundant power, networking, and connectivity.
- Isolated to avoid disasters.
- Connected with high bandwidth, ultra-low latency networking.

## IAM

- Identity and Access Management
- has a global view
- Never use root account, never write credentials in code
- **Users**: usually a physical person, one per person
- **Groups**: contains users
- **Roles**: internal usage within AWS resource, for machines, one per app
- Policies are in JSON, has redefined managed policies
- Best to give users the minimal amount of permissions (least privilege principle)
- Identity Federation uses the SAML standard

## EC2

- Consists of
  - Renting virtual machines (EC2)
  - Storing data on virtual drives (EBS)
  - Distributing load across machines (ELB)
  - Scaling the services using an auto-scaling group (ASG)
- In order to SSH into the instance
  - if on Linux: `chmod 0400 <key file name>` before `ssh -i <key file name> ec2-user@<public ip>`
  - if on Windows 10: add the user as the owner, add full control to the user, and then remove all control from other users or groups. Then SSH
- **Security groups**
  - regulate:
    - access to ports
    - authorised IP ranges / other security groups
    - control of inbound / outbound network
  - wrong security group settings may lead to **connection timeout** error
  - in contrast, 'connection refused' errors mean that the traffic has reached the instance
  - can be attached to multiple instances
  - one instance can have multiple SGs
  - Locked down to a region / VPC combination
  - by default:
    - All **inbound** traffic is **blocked**
    - All **outbound** traffic is **authorised**
- **Elastic IP** does not change as long as not deleted
- Use **EC2 User Data** to bootstrap the instance
- **EC2 Instance Launch Types**
  - **On Demand**: short workload, predictable pricing, highest price, paid **per second** after the first minute
  - **Reserved**: 
    - **Reserved Instances**: long workloads, eg database, up to 75% discount, long term commitment
    - **Convertible Reserved Instances**: long workloads with flexible instances, up to 54% discount
    - **Scheduled Reserved Instances**
  - **Spot Instances**: short workloads, for cheap, can lose instances, up to 90% discount, suitable for:
    - batch jobs
    - data analysis
    - image processing
  - **Dedicated Instances**: no other customers share hardware, but may share hardware with other instances in same account, *no control over placement*
  - **Dedicated Hosts**: an entire physical server, **control placement**, visibility into sockets/physical cores, **3 years reservation**, useful for licensing purposes / compliance needs
  - **Spot Fleets**: set of spot instances + (optional) on-demand instances
    - will try to meet the target capacity with price constraints
    - allow us to automatically request spot instances with the lowest price
    - strategies:
      - lowest price: from the pool with the lowest price, for cost optimization, short workload
      - diversified: distributed across all pools, for availability, long workloads
      - capacity optimized: pool with the optimal capacity for the number of instances
- **EC2 Instance Types**
  - **R** for RAM: in-memory caches
  - **C** for **CPU**: compute / databases
  - **M** for **Medium**: general / web app
  - **I** for **I/O**: databases
  - **G** for **GPU**: video rendering / machine learning
  - **T2 / T3**: burstable, - unlimited: unlimited burst

- **AMI**
  - motivation:
    - Pre-installed packages
    - **Faster boot time**
    - Machine comes configured with monitoring / enterprise software
    - Controls over the machines in the network
    - Control of maintenance and updates of AMIs over time
    - Active Directory Integration out of the box
    - Installing app ahead of time
    - Use other people's AMI that is optimised for certain job
  - **Specific to a region**
  - Charged for S3
  - Can share AMI with another account, does not change ownership
  - If copy an AMI, then becomes the owner of the copied AMI
  - Cannot copy an AMI with an associated billingProduct (Windows AMIs, AMIs from the Marketplace), should copy and recreate

- **EC2 Placement Groups**
  - controls how EC2 instances are placed
  - **Cluster**: grouped in a single AZ, low-latency
    - great network
    - fail at the same time
    - big data that needs to be complete fast
    - application that needs extremely low latency and high network throughput
  - **Spread**: across underlying hardware, max 7 per group per AZ, critical applications
    - reduced risk
    - limited to 7 instances per group
    - high availability
    - critical applications
  - **Partition**: across partitions within an AZ, scales to 100s of EC2 per group
    - up to 7 partitions per AZ
    - up to 100s of EC2 instances
    - a partition failure can affect many EC2 but not other partitions
    - EC2 instances get access to the partition information as metadata
    - Use cases: HDFS, HBase, Cassandra, Kafka
- **ENI - Elastic Network Interfaces**
  - Logical component in a VPC that represents a virtual network card
  - Attributes
    - One primary private IPv4, one or more secondary IPv4
    - One Elastic IP per private IPv4
    - One or more SGs
    - A MAC address
  - Can create ENI independently and attach on the fly on EC2
  - Bound to AZ

- **EC2 Hibernate**
  - The in-memory (RAM) state is preserved
  - boot is much faster
  - The RAM state is written to a file in the root EBS volume
  - The root volume must be EBS, encrypted, not instance store, and large