---
layout: post
title: 'AWS CloudFormation Basics: Usage with Examples'
date: '2023-06-19 12:56:35 +0530'
categories: [AWS, CloudFormation, Infrastructure as Code]
tags: [AWS, CloudFormation, Infrastructure as Code, Tags]
---
AWS CloudFormation is a powerful service that allows you to define and provision your AWS infrastructure as code. With CloudFormation, you can create and manage resources in a reliable and automated manner, ensuring consistency and repeatability across environments. In this post, we will explore the basics of AWS CloudFormation and provide examples to help you get started.

## Prerequisites

Before getting started, make sure you have the following:

1. An AWS account - Sign up for an AWS account if you don't have one already.
2. AWS Command Line Interface (CLI) - Install and configure the AWS CLI on your local machine.

## Step 1: Anatomy of a CloudFormation Template

A CloudFormation template is written in JSON or YAML format and describes the desired state of your AWS resources. It consists of the following key components:

- **Resources**: Defines the AWS resources you want to create, such as EC2 instances, S3 buckets, or DynamoDB tables.
- **Parameters**: Allows you to provide input values to your CloudFormation template, such as instance sizes or resource names.
- **Mappings**: Provides a mapping of keys to corresponding values that can be used in your template.
- **Conditions**: Enables conditional resource creation based on specific criteria.
- **Outputs**: Specifies the values you want to extract from your stack, such as the URL of a load balancer or the ID of a database.

## Step 2: Creating a Simple CloudFormation Stack

Let's start by creating a simple CloudFormation stack that provisions an EC2 instance. Here's an example CloudFormation template in YAML format:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: ami-0c94855ba95c71c99
      InstanceType: t2.micro
```

Save the above template in a file named `ec2-stack.yaml`. Now, execute the following command to create the CloudFormation stack:

```bash
aws cloudformation create-stack --stack-name my-ec2-stack --template-body file://ec2-stack.yaml
```

Wait for the stack creation to complete by running the following command:

```bash
aws cloudformation wait stack-create-complete --stack-name my-ec2-stack
```

Once the stack creation is complete, you can check the status and details of the stack:

```bash
aws cloudformation describe-stacks --stack-name my-ec2-stack
```

## Step 3: Adding Parameters and Outputs

Let's enhance our CloudFormation template by adding parameters and outputs. Parameters allow us to provide input values when creating the stack, while outputs allow us to extract useful information from the stack.

Here's an updated version of the CloudFormation template with parameters and outputs:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    Description: EC2 instance type
Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: ami-0c94855ba95c71c99
      InstanceType: !Ref InstanceType
Outputs:
  InstanceId:
    Value: !Ref MyEC2Instance
    Description: ID of the EC2 instance
```

Save the updated template in a file named `ec2-stack-params.yaml`. Now, create the

 stack with the following command:

```bash
aws cloudformation create-stack --stack-name my-ec2-stack --template-body file://ec2-stack-params.yaml --parameters ParameterKey=InstanceType,ParameterValue=t2.small
```

Wait for the stack creation to complete and then describe the stack:

```bash
aws cloudformation describe-stacks --stack-name my-ec2-stack
```

You can also extract the output values of the stack:

```bash
aws cloudformation describe-stacks --stack-name my-ec2-stack --query "Stacks[0].Outputs"
```

## Conclusion

AWS CloudFormation simplifies the provisioning and management of AWS resources by allowing you to define your infrastructure as code. In this post, we covered the basics of CloudFormation and demonstrated how to create a simple stack with parameters and outputs. By leveraging CloudFormation, you can achieve infrastructure automation, version control, and consistent resource provisioning across different environments.