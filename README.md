# AWS CloudFormation Networking Lab

## Overview

This project is an AWS Infrastructure as Code (IaC) lab built with **AWS CloudFormation**.

I started with an S3 bucket and expanded the project into a small AWS networking environment. The infrastructure is defined in YAML and deployed and updated using the AWS CLI.

The goal was to practice AWS networking, security, CloudFormation, infrastructure updates, and troubleshooting.

## AWS Services

* AWS CloudFormation
* Amazon VPC
* Amazon S3
* EC2 Security Groups
* Internet Gateway
* Route Tables
* Subnets
* AWS CLI

## Architecture

```text
                         AWS
                          │
                   CloudFormation
                          │
                          ▼
                     MyVPC
                  10.0.0.0/16
                          │
                    ┌─────┴─────┐
                    │           │
                    ▼           ▼
               MySubnet    MySecurityGroup
              10.0.1.0/24
                    │
                    ▼
               MyRouteTable
                    │
              0.0.0.0/0
                    │
                    ▼
          MyInternetGateway
                    │
                    ▼
                 Internet
```

The CloudFormation stack also creates an S3 bucket.

## CloudFormation Resources

| Logical ID                      | AWS Resource                   |
| ------------------------------- | ------------------------------ |
| `MyBucket`                      | S3 Bucket                      |
| `MyVPC`                         | VPC                            |
| `MySubnet`                      | Subnet                         |
| `MyInternetGateway`             | Internet Gateway               |
| `MyVPCGatewayAttachment`        | VPC Gateway Attachment         |
| `MyRouteTable`                  | Route Table                    |
| `MyInternetRoute`               | Route                          |
| `MySubnetRouteTableAssociation` | Subnet Route Table Association |
| `MySecurityGroup`               | Security Group                 |

## Networking

The project creates:

* A VPC using `10.0.0.0/16`
* A subnet using `10.0.1.0/24`
* A custom route table
* A default route using `0.0.0.0/0`
* An Internet Gateway
* A subnet-to-route-table association

The subnet's route table sends internet-bound traffic through the Internet Gateway.

## Security Group

A Security Group is created inside the project VPC:

```yaml
MySecurityGroup:
  Type: AWS::EC2::SecurityGroup
  Properties:
    GroupDescription: Security group for the learning VPC
    VpcId: !Ref MyVPC
```

No inbound rules were added because this project does not deploy an EC2 instance. The Security Group can be used by resources such as EC2 in a future project.

## Parameters

The template uses an `EnvironmentName` parameter for resource naming and tagging:

```yaml
EnvironmentName:
  Type: String
  Default: Learning
```

This allows the environment name to be changed without modifying the resource definitions.

## Deployment

The CloudFormation template was validated and deployed using the AWS CLI.

Validation:

```bash
aws cloudformation validate-template \
  --template-body file://template.yaml \
  --no-cli-pager
```

Stack status:

```bash
aws cloudformation describe-stacks \
  --stack-name cloudformation-lab-two \
  --query "Stacks[0].StackStatus" \
  --output text \
  --no-cli-pager
```

Resources:

```bash
aws cloudformation list-stack-resources \
  --stack-name cloudformation-lab-two \
  --no-cli-pager
```

## Troubleshooting

During development, I practiced troubleshooting both CloudFormation template errors and AWS deployment failures.

CloudFormation stack events were used to identify failed resources and the reason for failure:

```bash
aws cloudformation describe-stack-events \
  --stack-name cloudformation-lab-two \
  --no-cli-pager
```

This project reinforced that a template can pass validation but still fail during deployment because of AWS permissions or resource configuration.

## Skills Demonstrated

* Infrastructure as Code with AWS CloudFormation
* AWS VPC networking
* Subnets and CIDR blocks
* Route tables and routes
* Internet Gateway configuration
* Security Groups
* CloudFormation parameters
* AWS CLI
* Infrastructure troubleshooting
* Git and GitHub

## Project Structure

```text
cloudformation-lab-two/
├── template.yaml
└── README.md
```

## Cleanup

The CloudFormation stack can be removed when the project is no longer needed:

```bash
aws cloudformation delete-stack \
  --stack-name cloudformation-lab-two
```

