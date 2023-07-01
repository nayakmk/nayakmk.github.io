---
layout: post
title: AWS Lambda Execution Policy vs Resource Policy
date: '2023-07-01 16:58:13 +0530'
tags: [AWS Lambda, Serverless, Security, IAM]
categories: [Serverless, AWS Lambda]
---

When working with AWS Lambda, it's important to understand the concepts of execution policies and resource policies. These policies allow you to control the permissions and access rights for your Lambda functions and associated resources. In this post, we will explore the differences between execution policies and resource policies and when to use each.

## Execution Policies

Execution policies in AWS Lambda are similar to IAM policies and are attached to the execution role associated with your Lambda function. They define the permissions granted to the function's execution role, specifying what actions it can perform on AWS resources. Execution policies are evaluated at runtime and apply to all invocations of the function.

Key points about execution policies:

- Execution policies are attached to the execution role, not directly to the Lambda function.
- They control the permissions of the function's execution role to interact with AWS resources.
- Execution policies are evaluated at runtime.
- Changes to execution policies take effect immediately.

Example of an execution policy in JSON format:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:PutItem",
        "dynamodb:GetItem"
      ],
      "Resource": "arn:aws:dynamodb:us-east-1:123456789012:table/MyTable"
    },
    {
      "Effect": "Allow",
      "Action": "lambda:InvokeFunction",
      "Resource": "*"
    }
  ]
}
```

In the above example, the execution policy allows the Lambda function's execution role to perform `PutItem` and `GetItem` actions on a DynamoDB table, as well as invoke any Lambda function.

## Resource Policies

Resource policies in AWS Lambda are used to control access to individual Lambda functions. They allow you to define permissions for specific AWS identities or principals, such as IAM users, roles, or other AWS accounts. Resource policies are attached directly to the Lambda function and provide fine-grained control over who can invoke the function and under what conditions.

Key points about resource policies:

- Resource policies are attached directly to the Lambda function.
- They control access to the function by specifying which identities or principals can invoke it.
- Resource policies are evaluated before execution policies.
- Changes to resource policies may take some time to propagate.

Example of a resource policy in JSON format:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:user/MyUser"
      },
      "Action": "lambda:InvokeFunction",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:MyFunction"
    }
  ]
}
```

In the above example, the resource policy allows the IAM user "MyUser" to invoke the "MyFunction" Lambda function.

## Choosing between Execution Policies and Resource Policies

To decide whether to use execution policies or resource policies, consider the following factors:

- **Function-specific access**: If you need to grant permissions to specific AWS identities or principals for a particular Lambda function, resource policies provide more granular control.
- **Role-based access**: If you want to control permissions for multiple Lambda functions that share the same execution role, using execution policies on the execution role simplifies management.
- **Dynamic access control**: If you need to grant access to resources conditionally based on runtime variables or context, execution policies are more suitable as they are evaluated at runtime.



In some cases, you may need to use both execution policies and resource policies together to achieve the desired level of access control.

Understanding the differences between execution policies and resource policies helps you design secure and well-controlled AWS Lambda environments.

Stay tuned for more posts on AWS Lambda and serverless computing!

