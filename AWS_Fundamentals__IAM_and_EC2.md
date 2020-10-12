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

