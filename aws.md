# AWS

## VPC

- A VPC has subnets in multiple Availability Zones.
- Subnets can be private or public.
- "AZ A" of an AWS account can be phisically located differently from "AZ A" of another account.

## NAT Gateway

https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html

- EC2 instances without public IPs need some kind of NAT to access the internet (if not IPv6). NAT Gateway is one of them.
- Each availability zone should have a NAT Gateway.
- NAT Gateway has a private IP and a public IP.
- If NAT Gateway cost is too high, consider putting EC2 instances in a public subnet of VPC.

## VPC Flow Log

https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/flow-logs.html

- Each line represents a statistics of IP traffic between two pairs of host and port in **a second**.
- It's pretty low-level. We need a tool to analyze it.
- Need a Network Interface ID to find the right log file. If you use autoscaling, record the mapping of intance ID, private IP, (public IP if exists) and Network Interface ID somewhere. Otherwise the mapping is lost and you cannot look back.
- Keep in mind that IP address is considered as personal data

## Load Balancer

https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html

- Each Availability Zone should have a load balancer. Traffic to each of them is balanced by DNS.
- Cross-zone load balancing:
  - ALB: Always enabled
  - Classic ELB: Configurable
- Access log is opt-in.
  - To find load balancer 5xx: `grep ' 5[0-9]\{2\} - ' ${log_file}`
