Think of your application as just the core logic that connects to other AWS services

AWS is a "web" of services

EC2 - service that provides servers for your app
  - forms the basis for other apps like RDB service and Elastic Beanstalk

  AWS Tooling
  - web console
  - CLI 
  - AWS SDKs
    - .NET, Ruby, iOS, JS, Java, etc. 
  
AWS access key gives access for SDK and CLI

`aws configure` will allow you to use an access key on the CLI

`aws ec2 describe-instances`

## CloudWatch

Lets you set alarms for various thresholds
- billing estimates
- DB usage
- CPU usage

## Simple Notification Service

Topic -> unique amazon resource name

IAM -> Access Management
- passwords
- multi-factor auth
- access keys
- SSH keys

IAM Policy - collection of permissions for access to various services

## Policies

Effect - "Allow" or "Deny"
Action - what the user can do
Resource - thing action is performed on