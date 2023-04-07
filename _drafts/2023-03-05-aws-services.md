---
layout: post
title: 'AWS: Services'
date: '2023-03-05 11:25:22 +0530'
---

## Services

### Networking

| Service Type | Service Name    | Description | Diagram                                                      |
| ------------ | --------------- | ----------- | ------------------------------------------------------------ |
| DNS          | Amazon Route 53 |             | ![img](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2021/06/28/illustration.png) |

### Logging and Monitoring

| Service Type             | Service Name                  | Description                                                  | Diagram                                                      |
| ------------------------ | ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Logging                  | AWS CloudTrail                | AWS CloudTrail is a service that provides a record of AWS API calls and events for auditing, compliance, and troubleshooting purposes. | ![AWS CloudTrail \| AWS Architecture Blog](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2021/07/21/Figure-1.-Fitness-functions-provide-feedback-to-engineers-via-metrics-1024x508.png) |
| Monitoring               | AWS CloudWatch                | AWS CloudWatch is a monitoring service that provides metrics, logs, and alarms for AWS resources and applications running on AWS. | ![Centralized Logging on AWS \| AWS Solutions](https://d1.awsstatic.com/aws-centralized-logging-architecture.b64ae5f974d59c24ab9633c1565b27c7ad5dc0c8.b64ae5f974d59c24ab9633c1565b27c7ad5dc0c8.png) |
|                          | CloudWatch Logs               | CloudWatch Logs is a service that enables you to monitor, store, and access your log files from Amazon EC2 instances, AWS CloudTrail, or other sources. You can use CloudWatch Logs to track the number of errors that occur in your application logs and send you a notification whenever the rate of errors exceeds a threshold you specify. You can also use CloudWatch Logs Insights to search and analyze log data interactively. |                                                              |
|                          | CloudWatch EventBridge        | CloudWatch EventBridge is the latest version of CloudWatch Events, which is a serverless event bus service of AWS. It allows you to match events from AWS services and your own applications and route them to one or more targets, such as Lambda functions, SNS topics, or SQS queues. EventBridge provides more features than CloudWatch Events, such as integration with third-party services, schema discovery, and event replay. |                                                              |
|                          | CloudWatch Insights           | AWS CloudWatch Insights is a service offered by AWS to search and analyze log data interactively. It enables users to query logs to help determine the potential causes of operational issues and resolve them. It is a centralized tool to analyze logs and metrics from all combined AWS resources¹. It is a pay-as-you-go service, which means no billing for non-usage¹. All AWS features, such as auto-scaling resources, are integrated with CloudWatch Insights. |                                                              |
|                          | AWS X-Ray                     |                                                              |                                                              |
|                          | Amazon CodeGuru               |                                                              |                                                              |
| Service Health Dashboard | AWS Status                    | Shows all regions, all services health  Shows historical information for each day |                                                              |
|                          | AWS Personal Health Dashboard | While the Service Health Dashboard displays the general status of AWS services, Personal Health Dashboard gives you a personalized view into the performance and availability of the AWS services underlying your AWS resources. |                                                              |
|                          |                               |                                                              |                                                              |

### Configuration and Compliance

| Service Type                 | Service Name        | Description                                                  | Diagram                                                      |
| ---------------------------- | ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Configuration and Compliance | AWS Config          |                                                              | ![This diagram shows how AWS Config continuously tracks the state of resources in your account. When changes are detected, AWS Config tracks records those changes and maintains a history. Those changes and history are delivered to an s3 bucket and can be later accessed via the console or the API. If a rule is deployed to evaluate the resource, it can be triggered automatically. The evaluation results can be displayed on the console or accessed via the AWS Config API. ](https://d2908q01vomqb2.cloudfront.net/972a67c48192728a34979d9a35164c1295401b71/2020/09/17/AWS-Config-Workflow3.png) |
|                              | AWS Trusted Advisor | AWS Trusted Advisor **provides recommendations that help you follow AWS best practices**. Trusted Advisor evaluates your account by using checks. These checks identify ways to optimize your AWS infrastructure, improve security and performance, reduce costs, and monitor service quotas. |                                                              |
|                              |                     |                                                              |                                                              |

### Storage and Analytics

| Service Type | Service Name                                          | Description     | Diagram |
| ------------ | ----------------------------------------------------- | --------------- | ------- |
| SQL          | Relational Database Service (RDS)                     |                 |         |
| SQL          | Amazon Aurora                                         |                 |         |
| Document DB  | Amazon DocumentDB                                     | Managed MongoDB |         |
| Key Value DB | Amazon DynamoDB                                       |                 |         |
| Graph DB     | Amazon Neptune                                        |                 |         |
| Caching      | AWS ElastiCache and Amazon DynamoDB Accelerator (DAX) |                 |         |
| Hadoop       | Elastic MapReduce                                     |                 |         |
| Warehouse    | Amazon Redshift                                       |                 |         |
| Analytics    | Amazon Athena                                         |                 |         |
| Dashboarding | QuickSight                                            |                 |         |
| Ledger DB    | Amazon QLDB                                           |                 |         |
| ETL          | Amazon Glue                                           |                 |         |
| Blockchain   | Amazon Managed Blockchain                             |                 |         |
| Migration    | Amazon Data Migration Service (DMS)                   |                 |         |



