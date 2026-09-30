# AWS CloudFormation Lab

## Overview

This project demonstrates the use of **AWS CloudFormation** to provision and manage AWS infrastructure using Infrastructure as Code (IaC).

The project uses a YAML-based CloudFormation template to create and manage an Amazon S3 bucket. The template was validated, deployed as a CloudFormation stack, updated, and then configured to accept an environment parameter.

## AWS Services & Technologies

- AWS CloudFormation
- Amazon S3
- AWS CLI
- YAML

## Architecture

```text
template.yaml
      |
      v
AWS CloudFormation
      |
      v
CloudFormation Stack
"cloudformation-lab-two"
      |
      v
Amazon S3 Bucket
```

## CloudFormation Resources

### Amazon S3 Bucket

The CloudFormation template defines an S3 bucket as a resource:

```yaml
MyBucket:
  Type: AWS::S3::Bucket
```

CloudFormation is responsible for creating and managing the bucket as part of the stack.

### Environment Parameter

The template uses an `EnvironmentName` parameter:

```yaml
Parameters:
  EnvironmentName:
    Type: String
    Default: Learning
```

The parameter allows an environment value to be provided when the stack is deployed.

The value is used as a tag on the S3 bucket:

```yaml
Tags:
  - Key: Environment
    Value: !Ref EnvironmentName
```

For this project, the stack was updated using `Development` as the environment value.

## Deployment Workflow

The project followed this workflow:

1. Created a CloudFormation YAML template.
2. Validated the template using the AWS CLI.
3. Created a CloudFormation stack.
4. Verified the stack reached `CREATE_COMPLETE`.
5. Inspected the resources created by the stack.
6. Updated the template to add an `Environment` tag.
7. Updated the CloudFormation stack.
8. Added an `EnvironmentName` parameter to make the template configurable.
9. Updated the stack using `Development` as the parameter value.
10. Verified the stack reached `UPDATE_COMPLETE`.

## What I Learned

This project helped me understand the relationship between CloudFormation templates, stacks, resources, and parameters.

Key concepts practiced:

- **CloudFormation** — AWS service used to define and manage infrastructure as code.
- **Template** — YAML file that describes the AWS resources CloudFormation should create and manage.
- **Stack** — Deployed collection of AWS resources managed by CloudFormation.
- **Resource** — An individual AWS service or component defined within a CloudFormation template.
- **Parameter** — An input value that can be provided when deploying or updating a CloudFormation stack.

The project also provided hands-on experience with validating, deploying, updating, and verifying AWS infrastructure using the AWS CLI.
