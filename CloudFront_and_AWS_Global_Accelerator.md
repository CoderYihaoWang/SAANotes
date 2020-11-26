# CloudFront & AWS Global Accelerator

## CloudFront

- Content Delivery Network (CDN)
- content cached at the edge (216)
- DDoS protection, integration with Shield, AWS Web App Firewall 
- can expose external HTTPS and can talk to internal HTTPS backends
- Origins:
  - S3 bucket
    - for distributing files
    - enhanced security with CloudFront Origin Access identity (OAI)
    - can upload from CloudFront to S3
  - Custom Origin (HTTP)
    - ALB (must be public)
    - EC2 (must be public)
    - S3 website
- Geo restriction
  - whitelist / blacklist
  - for copyright laws etc
- CloudFront vs S3 Cross Region Replication
  - CloudFront
    - global edge network
    - File cached for TTL
    - great for static content that must be available everywhere
  - S3 Cross Region Replication
    - must be set up for each region of replication
    - near real-time
    - read only
    - great for dynamic content that needs to be available at low-latency in few regions

- CloudFront pre-signed URLs / cookie
  - includes URL expiration
  - includes IP ranges to access the data from
  - trusted signers
  - shared content -> short, private content -> long
  - pre signed URL -> individual files
  - pre signed cookies -> multiple files
  - CloudFront pre signed URL vs S3 pre signed URL
    - CloudFront:
      - access to path, no matter the origin
      - account wide key-pair, only the root can mange it
      - can filter by IP, path, date, expiration
      - can leverage caching features
    - S3
      - issue a request as the person who pre-signed the URL
      - uses the IAM key of the signing IAM principal
      - limited lifetime

## Global Accelerator

- go as fast as possible through AWS network to minimize latency
- Unicast IP: one server holds one IP
- Anycast IP: all servers hold the same IP address and the client is routed to the nearest one
- leverage the AWS internal network to route to apps
- 2 Anycast IP are created for app
- the Anycast IP send traffic directly to Edge Locations
- the Edge locations send the traffic to app
- works with Elastic IP, EC2 instances, ALB, NLB, public or private
- consistent performance
- health checks
- AWS Global Accelerator vs CloudFront
  - same AWS global network and its edge locations
  - both integrate with AWS Shield for DDoS
  - CloudFront
    - improves performance for both cacheable content and dynamic content
    - content served at the edge
  - Global Accelerator
    - improves performance for a wide range of applications over TCP or UDP
    - proxying packets at the edge to apps running in one or more AWS regions
    - good for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP
    - good for HTTP use cases that require static IP addresses
    - good for use cases that required deterministic, fast regional failover

 
