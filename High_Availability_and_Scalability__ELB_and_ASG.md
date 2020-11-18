# High Availability and Scalability: ELB & ASG

## Scalability

- An application / system can handle greater loads by adapting
- **Vertical scalability**: increase the size of the instance, common for non-distributed systems (ie RDS, ElastiCache)
- **Horizontal scalability (elasticity)**: increase the number of instances, for distributed systems (ie web app)
- High availability means running app/system in at least 2 data centers (az's)

## Elastic Load Balancers (ELB)

- Servers that forward internet traffic to multiple servers
- Why load balancers:
  - Spread load across instances
  - Expose a single DNS
  - Seamlessly handle failures of instances
  - Regular health checks
  - Provide SSL termination for websites
  - Enforce stickiness with cookies
  - High availability across zones
  - Separate public traffic from private traffic

- Health checks done on a port and a route
- 503 errors indicates lack of capacity or targets

- Three types:
  - **Classic Load Balancer (CLB)**
    - HTTP, HTTPS (Layer 7), TCP (Layer 4)
    - Health checks are TCP or HTTP based
    - Fixed hostname
  - **Application Load Balancer (ALB)** 
    - HTTP, HTTPS, HTTP/2, WebSocket (Layer 7)
    - Multiple applications across machine or within the same machine
    - Support redirect
    - Routing tables to different target groups (based on paths, hostnames, query string, headers)
    - Great for micro services and container-based application
    - Port mapping feature to redirect to a dynamic port in ECS
    - Target groups: EC2 instances, ECS tasks, Lambda functions, IP addresses
    - Fixed hostname
    - Application servers don't see the IP of the client directly
  - **Network Load Balancer (NLB)**
    - TCP, TLS, UDP (Layer4)
    - fast
    - One static IP per AZ and supports assigning Elastic IP

- **Stickiness**: the same client is always redirected to the same instance (CLB and ALB)
- **Cross-zone load balancing**: each load balancer instance distributes evenly across all registered instances in all az (always on for ALB, disabled by default for CLB and NLB, charges for NLB)
- **Server Name Indication (SNI)** for loading multiple SSL certificated onto one web server (works for ALB, NLB, CloudFront)

- **Connection draining**: aka deregister delay for target groups. Time to complete in-flight requests when de-registering. 300 seconds by default. 1 - 3600, can be disabled by setting to 0

## Auto Scaling Group (ASG)

- Attributes:

  - launch configuration
    - AMI + instance type
    - EC2 user data
    - EBS Volumes
    - Security groups
    - SSH key pair
  - min size / max size / initial capacity
  - network + subnets
  - load balancer info
  - scaling policies

- Auto scaling alarms

  - CloudWatch alarms
  - an alarm monitors a metric (ie average CPU)
  - metrics are computed for the overall ASG instances
  - custom metrics can be CPU, network schedule, so on

- Use launch configurations or launch templates

- Scaling policies

  - **target tracking scaling**: simple and easy to set-up
  - **simple / step scaling**

  - **scheduled actions**

- Scaling cooldowns:

  - ensure that ASG doesn't launch or terminate additional instances before the previous scaling activity takes effect
  - default is 300 seconds, can be over written

- Default termination policy
  - find the az which has the most number of instances
  - if there are multiple instances in the az to choose from, delete the one with the oldest launch configuration
  - tries to balance by az
- Lifecycle hooks: able to perform extra steps before the instance goes in service (pending state), and before the instance is terminated (terminating state)