# Automate your app with Elastic Beanstalk and CloudFormation

## CloudFormation

CloudFormation - Service to provision resources using templates

Template - key object in CloudFormation
- JSON document
- Contains config for resources
- Can be used in VCS

Deleting a stack removes all resrouces created in that stack

CloudFormation only creates the resources you explicitly configure in the template

Tools
- CloudFormation designer
- CloudFormer - creates a template based on existing infrastrcture

## Elastic Beanstalk

Scaling
Monitoring
Resource Provisioning

### Structures

- Application
  - parent container for a logical applications
  - Runs on a single platform
  - Has one or more Application Versions

Worker environment - for long-running tasks
Web server environment - for web apps

Elastic Beanstalk uses CloudFormation to accomplish a lot of these tasks