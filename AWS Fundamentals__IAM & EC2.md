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
- A **global** service, across regions
- Never use root account
- Never put IAM credentials in code
- Policies are in JSON
- **Users**: one per physical person
- **Groups**: usually by functions / by teams...; contains users
- **Roles**: for internal usage within AWS resources; one per application
- **Least privilege principle**: only grant least amount of permission
- **IAM Federation**: login into AWS using company credentials; uses SAML

## EC2

