---
layout: post
title: 'AWS: Cloud Practitioner - Services Buzz Words'
date: '2023-04-20 11:48:42 +0530'
---

## Compute

### Reserved Instances

- AWS Reserved Instances (RIs) provide customers with a discounted hourly rate for EC2 instances in exchange for a commitment to use a specific instance type, in a specific Availability Zone, for a specified period of time (1-3 years). 
- Reserved Instances are ideal for customers with predictable workloads who are committed to using a specific instance type for a period of time. 

### Dedicated Host vs Dedicated Instances

- AWS Dedicated Hosts provide dedicated physical servers to a single customer, 
- Dedicated Instances provide dedicated EC2 instances on shared physical servers.
- AWS Dedicated Hosts allows you address compliance requirements and **use your existing server bound software licenses (per-socket, per-core, per - VM software licenses)**

### Capacity Reservations

Capacity Reservations, on the other hand, allow customers to reserve capacity for EC2 instances in a specific Availability Zone, without having to commit to a specific instance type or a specific period of time .

- Suitable for short-term, uninterrupted workloads that needs to be in a specific AZ

## Storage

### High Availability By Default

- Amazon S3

- Amazon EFS

### Multi AZ Replication using Cloud Native Storage

- Amazon RDS for Oracle
- Amazon Neptune 
- Amazon S3

- Amazon EBS

- Amazon Aurora

- Amazon DynamoDB

- Amazon EFS

- Amazon Neptune

### Amazon FSx for Windows File Server  vs Amazon Elastic File Service

- Amazon FSx for Windows File Server is designed for use cases such as **Microsoft SQL Server, Microsoft SharePoint, and home directories**. 
- Amazon EFS is designed for use cases such as **content management, big data analytics, and web serving**. 
- Amazon FSx for Windows File Server supports the **SMB** protocol, while Amazon EFS supports the **NFS** protocol.
- Amazon FSx for Windows File Server is designed for use with **Windows-based** workloads, while Amazon EFS is designed for use with **Linux-based** workloads. 
- Amazon FSx for Windows File Server provides **cross-AZ replication**, while Amazon EFS provides **cross-region replication**.
- Amazon FSx for Windows File Server **supports encryption at rest and in transit**, while Amazon EFS supports **encryption at rest** 

### Automated Data Backup

- Amazon Aurora

## Streaming Analytics

- Amazon Kinesis provides a suite of tools and services that allow you to build real-time applications that can process and analyze the data in real-time. 
- Additionally, Kinesis enables you to respond to customer queries in real time using this data. 

## Log Analysis

- **Amazon CloudWatch Logs Insights** allows you to search, analyze, and visualize log data from multiple sources in near real-time.

## Log Streaming

Amazon CloudWatch Logs streams 

allows you to send log data from your applications and services to CloudWatch Logs in near real-time. Streams are used to organize and separate log data from different sources and can be configured to route to different destinations such as Amazon S3, Amazon Elasticsearch, and AWS Lambda functions.

## Log Anomaly Detection

Amazon CloudWatch anomaly detection 

uses machine learning algorithms to automatically detect anomalies in your metrics data. It can help you identify unexpected behavior and potential problems in your systems, and take action before they become critical. 

## Networking

### Virtual Private Cloud End Point Gateway

- S3
- DynamoDB

### Virtual Private Cloud End Point Interface

Rest all the services

## Security

### AWS Web Application Firewall

- **Layer 7 (HTTP)** Protection from Web Exploits
- Deploy on **Application Load Balancer, API Gateway, CloudFront**
- Define Web ACL to protect against
  - **SQL injection and Cross-Site Scripting (XSS)**
  - **geo-match (block countries)**
  - **DDoS protection**

### AWS Certificate Manager

- Let’s you easily provision, manage, and deploy **SSL/TLS Certificates**
- Used to provide **in-flight encryption for websites** (HTTPS)
- Supports both public and private TLS certificates

### Compliance Reports

AWS Artifact

### AWS Abuse



## Application Development

### Amazon Cognito

Amazon Cognito is a managed service offered by AWS that provides user sign-up, sign-in, and access control for mobile and web applications:

- User Management 
- User Authentication 
- Authorization 
- Scalability and Availability 

## Billing Services

### AWS Budgets

- Create budget and **send alarms when costs exceeds the budget**

### Cost Explorer

- Visualize, understand, and manage your AWS costs and usage over time
- **Forecast usage up to 12 months based on previous usage**

## Infrastructure

### AWS Outpost

### AWS Local Zone

## Disaster Recovery

- Backup and Restore
- PilotLight
- Warm Standby
- Multi Site

## Management

### AWS Shared Responsibility Model

**Inherited Controls** – Controls which a customer fully inherits from AWS.

- Physical and Environmental controls

**Shared Controls** – Controls which apply to both the infrastructure layer and customer layers, but in completely separate contexts or perspectives. In a shared control, AWS provides the requirements for the infrastructure and the customer must provide their own control implementation within their use of AWS services. Examples include:

- **Patch Management** – AWS is responsible for patching and fixing flaws within the infrastructure, but customers are responsible for patching their **guest OS and applications**.
- **Configuration Management** – AWS maintains the configuration of its infrastructure devices, but a customer is responsible for configuring their own guest operating systems, databases, and applications.
- **Awareness & Training** - AWS trains AWS employees, but a customer must train their own employees.

**Customer Specific** – Controls which are solely the responsibility of the customer based on the application they are deploying within AWS services. Examples include:

- Service and Communications Protection or Zone Security which may require a customer to route or zone data within specific security environments.

### AWS Managed Services

### Service Quota

- AWS Service Quotas
- AWS Trusted Advisor

## Migration

### AWS Server Migration Service

Using **AWS Direct Connect** with **AWS Server Migration Service** allows you to transfer large amounts of data between your on-premises data center and your AWS account more quickly and reliably than over the public internet. 

## Support Plans

### Programmatic Case Management

Business Plan

### One Contact with Multiple Cases

Developer

### Multiple Contact with Multiple Cases

Business

### General Architectural Guidance

Developer

### Use Case wise Architecture Guidance

Business

### 24x7 Support

Business

### Business Hour Support

Developer

### Infrastructure Event Management

Business Plan with **Addition Cost**

Enterprise on Ramp - By Default

### 3rd Party Software Integration

Enterprise on Ramp

### Access to AWS Subject Matter Expert

Enterprise on Ramp

### Pool of Technical Account Manager

Enterprise on Ramp

### Dedicated Technical Account Manager

Enterprise

### Self Paced Labs and Online Training

Enterprise

### Concierge Support

Enterprise on Ramp

## Cloud Adaptation Framework

- **B**usiness
- **P**eople
- **G**overnance
- **P**latform
- **S**ecurity
- **O**perations

## Well Architected Framework

- CO : Cost Optimization
- OE: Operational Excellence
- PE: Performance Efficiency
- R: Reliability
- S: Security
- S: Sustainability