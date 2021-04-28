# Virtual Private Cloud

Allows logical isolation of cloud resources

Free service

Security group - defines allowed incoming/outgoing IP addresses and ports. Like a mini-firewall

## Subnets

Instances must be lauched into subnet in VPC

Has routing table and Network Access Control list

Availability zone - data centers that have super fast connections to each other but are physicaly separated

# Elastic Cloud Compute (EC2)

EC2 instance is a virtual machine
- CPU
- Memory
- Storage
- Network Performance

Amazon Machine Image (AMI) - OS and software installed on an EC2 instance

Different optimizations
- Storage
- CPU

Elastic Block Storage - Indepdendent storage volumes used with EC2 instances

Elastic IP - Public IP addresses that are created, destroyed, and assigned independently

Custom AMIs can be used

Auto Scaling Group - Expands or shrinks a pool of instances based on pre-defined rules

Load Balancer - Routing appliance that maintains a consistent DNS entry and balances requests to multiple instances

## S3

Stores objects (files)
Stores objects in region you specify

An object is basically a file and metadata

Objects are uniquely identified in s3 via their key:
- path
- filename (excluding bucket name)

Cross-region replication can solve latency issues or increase redundancy of data (CloudFront is better for latency)

Buckets have permissions

Policies can be configured for S3 buckets

The AWS CLI has the permissions given to the user whose key is configured on your system
