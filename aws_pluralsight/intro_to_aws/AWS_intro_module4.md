# Elastic Beanstalk EB

EB automates deployment & configuration

Aggregates monitoring and logging

Application verions are stored in S3

EB is free* - you pay for the EC2 instances, load balancers, and S3 separately

# DynamoDB

Unlimited, Elastic storage

No hardware choices

Pay only for what you use

Base structure - table
- Primary key
- Secondary indexes

Provisioned Throughput Capacity - # of read/write units per second

Pricing depends on Provisioned Throughput Capacity and amount of data stored

# RedShift

Cluster
- One or more nodes

Nodes have types

Security
- Can encrypt entire data warehouse
- VPC protection
- No public IP for cluster
  
  Dense compute vs dense storage

# Virtual Private Cloud

VPC is next evolution of security groups

Full control over routing tables

NAT gateways for outbound traffic

Multiple subnets within VPC
- public vs private

# CloudWatch

- Monitoring resources
- Acting on alerts

Define your own metrics and thresholds to set alerts on

Custom dashboards

# CloudFront

CDN - helps serve files the fastest based on location