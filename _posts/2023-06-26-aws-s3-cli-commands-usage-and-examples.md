---
layout: post
title: AWS S3 CLI Commands - Usage and Examples
date: '2023-06-26 22:36:01 +0530'
categories: [AWS, S3]
tags: [AWS, S3, CLI, Command Line]
---

In this post, we will explore various AWS S3 CLI commands that can be used to interact with Amazon S3 buckets and objects. The AWS CLI provides a convenient and powerful way to manage your S3 resources from the command line.

## Prerequisites

Before getting started, make sure you have the following set up:

1. An AWS account with appropriate permissions to access and manage S3 resources.
2. The AWS CLI installed and configured with your AWS credentials. You can install the AWS CLI by following the instructions in the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html).

## Common S3 CLI Commands

### List Buckets

To list all the S3 buckets in your account, use the following command:

```bash
aws s3 ls
```

### List Objects in a Bucket

To list all the objects in a specific S3 bucket, use the following command:

```bash
aws s3 ls s3://bucket-name
```

### Upload a File to a Bucket

To upload a file to an S3 bucket, use the following command:

```bash
aws s3 cp /path/to/local/file s3://bucket-name/path/to/destination/file
```

### Download a File from a Bucket

To download a file from an S3 bucket, use the following command:

```bash
aws s3 cp s3://bucket-name/path/to/source/file /path/to/local/destination
```

### Delete a File from a Bucket

To delete a file from an S3 bucket, use the following command:

```bash
aws s3 rm s3://bucket-name/path/to/file
```

### Create a Bucket

To create a new S3 bucket, use the following command:

```bash
aws s3 mb s3://bucket-name
```

### Delete a Bucket

To delete an empty S3 bucket, use the following command:

```bash
aws s3 rb s3://bucket-name
```

### Sync Local Directory with Bucket

To synchronize a local directory with an S3 bucket, use the following command:

```bash
aws s3 sync /path/to/local/directory s3://bucket-name
```

### Set Bucket ACL (Access Control List)

To set the ACL of an S3 bucket, use the following command:

```bash
aws s3api put-bucket-acl --bucket bucket-name --acl acl-value
```

### Enable Versioning for a Bucket

To enable versioning for an S3 bucket, use the following command:

```bash
aws s3api put-bucket-versioning --bucket bucket-name --versioning-configuration Status=Enabled
```

### Copy Objects between Buckets

To copy objects between S3 buckets, use the following command:

```bash
aws s3 cp s3://source-bucket/path/to/source/file s3://destination-bucket/path/to/destination/file
```

## Conclusion

AWS S3 CLI commands provide a convenient way to manage and interact with your S3 buckets and objects directly from the command line. These commands enable you to perform various operations such as listing buckets, uploading and downloading files, deleting objects and buckets, and more.

Remember to refer to the official [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/s3/index.html) for detailed information on each command and its options.

Feel free to customize the content, add more examples, or include any additional information to make the post more informative and comprehensive.

References:
- [AWS Command Line Interface (CLI)](https://aws.amazon.com/cli/)
- [AWS CLI Command Reference - Amazon S3](https://docs.aws.amazon.com/cli/latest/reference/s3/index.html)

