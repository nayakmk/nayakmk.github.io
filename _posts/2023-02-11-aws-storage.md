---
layout: post
title: 'AWS: Storage'
date: '2023-02-11 18:11:31 +0530'
categories: [AWS]
tags: [aws, storage]
---

## AWS Storage

Amazon Web Services (AWS) offers a variety of storage options to meet the needs of different types of applications and workloads. Some of the most commonly used AWS storage options include:

1. **Amazon Simple Storage Service (S3)**: A highly scalable, **object-based storage service** that allows you to store and retrieve any amount of data from anywhere on the internet**.**
2. **Amazon Instance Storage**: Instance store is also known as ephemeral storage. It is a type of non-persistent storage that is physically attached to the host computer of the EC2 instance. Instance store provides fast, low-latency access to data and is ideal for applications that require temporary storage of frequently changing data. However, the data stored on an instance store volume will be lost if the instance is stopped or terminated.
3. **Amazon Elastic Block Store (EBS)**: A block-level storage service that provides persistent storage for Amazon EC2 instances.
4. **Amazon Glacier**: A low-cost, long-term storage service that is well-suited for data archiving, backup, and disaster recovery.
5. **Amazon Elastic File System (EFS)**: A fully managed, elastic file storage service that makes it easy to set up, scale, and operate file systems in the cloud.
6. **Amazon S3 Transfer Acceleration**: A feature of S3 that enables fast, easy, and secure transfers of large files over long distances using Amazon CloudFront's globally distributed edge locations.
7. **AWS Storage Gateway**: A hybrid storage solution that allows you to store data in the AWS cloud while still retaining the performance of on-premises storage.

### AWS S3

#### Properties

Some of the most commonly used S3 bucket properties include:

1. **Bucket name**: A unique name that identifies your S3 bucket and its contents. Bucket names must be globally unique across all S3 users, so you'll need to choose a name that hasn't already been taken.
2. **Region**: The geographic region where your bucket will be stored. By choosing a region that is close to your users or application, you can reduce latency and improve performance.
3. **Access control**: The permissions that determine who can access your bucket and its contents. You can set up access controls to grant or restrict access to your bucket to specific AWS identities, such as users or groups, or to the public.
4. **Versioning**: A feature that allows you to store multiple versions of an object in the same bucket, so you can recover previous versions of your data in case of accidental deletion or overwrite.
5. **Lifecycle policies**: Rules that automatically transition objects to different storage classes or delete them based on their age, size, or other criteria.
6. **Cross-Region Replication**: A feature that allows you to replicate your S3 bucket and its contents to another region, for disaster recovery or for serving users from multiple regions.
7. **Server access logging**: A feature that logs all access requests to your bucket, so you can monitor and audit usage of your S3 data.
8. **Encryption**: The encryption method used to secure the contents of your bucket. You can choose to encrypt your data at rest using server-side encryption with Amazon S3-managed encryption keys (SSE-S3), or you can use client-side encryption with customer-provided encryption keys (SSE-C).
9. **Storage Class**: The storage class of an object determines its availability, durability, and cost.  

#### Storage Class

Amazon Simple Storage Service (S3) offers several storage options to meet the needs of different types of applications and use cases. Some of the most commonly used S3 storage options include:

1. **Standard**: A general-purpose storage class that provides low latency and high throughput performance.
2. **Intelligent-Tiering**: An automatic tiering storage class that moves objects between two access tiers (frequent and infrequent access) based on changing access patterns.
3. **Standard-IA (Infrequent Access)**: A low-cost storage class for infrequently accessed data.
4. **One Zone-IA**: A lower-cost storage class for infrequently accessed data that is stored in a single availability zone.
5. **Glacier**: A low-cost, long-term storage class for data archiving and backup.
6. **Glacier Deep Archive**: A secure, durable, and extremely low-cost storage class for long-term retention of data that is rarely accessed.
7. **Outposts**: An extension of S3 to on-premises environments, allowing you to store, process, and analyze data locally.

#### Lifecycle Rules

Amazon Simple Storage Service (S3) provides a feature called lifecycle rules, which allow you to automatically transition objects to different storage classes or delete them based on their age, size, or other criteria. Lifecycle rules are a cost-effective way to manage the storage of your S3 data and optimize your storage costs. 

Once we have created a lifecycle rule, S3 will automatically apply the rule to your objects based on the specified criteria. We can view the status of your lifecycle rules in the S3 Management Console, and we can modify or delete them at any time. 

### AWS EBS

#### Properties

Amazon Elastic Block Store (EBS) has several properties that make it a flexible and powerful storage solution for Amazon Elastic Compute Cloud (EC2) instances:

1. **Persistence**: EBS volumes are persistent, meaning that the data stored on an EBS volume will persist even if the EC2 instance is stopped or terminated. This makes EBS an ideal storage solution for applications that require durable, long-term storage.
2. **Block-level storage**: EBS volumes provide block-level storage, which allows EC2 instances to access data in a fine-grained, low-latency manner. This makes EBS a suitable storage solution for applications that require fast, random access to data.
3. **Volume Types**: EBS provides several volume types, each with different performance characteristics, to meet the varying storage needs of different applications. For example, you can choose from standard magnetic volumes for low-cost storage, solid-state drives (SSD) for high-performance storage, or provisioned IOPS (input/output operations per second) SSD for applications that require a consistent and high level of I/O performance.
4. **Snapshots**: EBS provides snapshots, which are point-in-time copies of an EBS volume. Snapshots can be used to create new volumes, backup data, or store data for disaster recovery. EBS snapshots are stored in Amazon Simple Storage Service (S3) and are automatically replicated within an Availability Zone for data durability.
5. **Encryption**: EBS provides encryption options for volumes, allowing you to encrypt data at rest for added security. You can choose from several encryption options, including encryption using an AWS Key Management Service (KMS) customer master key, or using the default encryption provided by Amazon S3.
6. **Backup and Recovery**: EBS provides automatic backups and recovery options, making it easy to create and manage backups of your data. You can choose to back up your data to Amazon S3 or another EBS volume, and you can also use snapshots to recover data in the event of a failure.

### AWS Elastic File Storage

Amazon Elastic File System (EFS) is a scalable, distributed file storage service provided by Amazon Web Services (AWS). EFS has several properties that make it a powerful and flexible storage solution:

1. **Scalability**: EFS provides scalable file storage, allowing you to easily adjust your storage capacity as your needs change. EFS automatically scales up and down as the number of files in your file system grows or shrinks.
2. **Accessibility**: EFS provides a single, shared file system that can be accessed from multiple Amazon Elastic Compute Cloud (EC2) instances simultaneously. This makes it easy to share data between instances and enables applications to run in a distributed, highly available manner.
3. **Durability**: EFS is built for high availability and durability, with data stored across multiple Availability Zones within an AWS region. This provides automatic data redundancy and protection against failures, ensuring that your data is always available.
4. **Performance**: EFS provides several performance options, allowing you to choose the level of performance that best meets the needs of your application. You can choose between performance classes based on the number of input/output operations per second (IOPS) required by your application.
5. **Security**: EFS provides several security options, allowing you to secure your data using Amazon Virtual Private Cloud (VPC) security groups and network access control lists (ACLs). You can also encrypt your data at rest using Amazon S3-managed encryption keys.
6. **Cost-Effective**: EFS provides cost-effective file storage, with charges based on the amount of data you store, the number of I/O operations performed, and the amount of data you transfer out of the service. You only pay for what you use, and there are no upfront costs or minimum commitments.