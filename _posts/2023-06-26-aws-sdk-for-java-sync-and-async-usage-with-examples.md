---
layout: post
title: AWS SDK for Java - Sync and Async Usage with Examples
date: '2023-06-26 22:53:33 +0530'
categories: [AWS, Java]
tags: [AWS, SDK, Java, Examples, Tags, Category]
---

In this post, we will explore the AWS SDK for Java and its usage for both synchronous (sync) and asynchronous (async) programming styles. The AWS SDK for Java provides a comprehensive set of APIs and libraries that allow developers to build applications that integrate with various AWS services.

## Prerequisites

Before getting started, make sure you have the following set up:

1. An AWS account with appropriate permissions to access and manage AWS resources.
2. Java Development Kit (JDK) installed on your machine.
3. Your favorite Java IDE or text editor.

## AWS SDK for Java Sync and Async Examples

### Setting Up AWS Credentials

To use the AWS SDK for Java, you need to provide your AWS credentials. You can configure the AWS credentials using the AWS CLI, environment variables, or directly in your code. Here's an example of setting up AWS credentials using the `DefaultAWSCredentialsProviderChain`:

```java
import software.amazon.awssdk.auth.credentials.DefaultAWSCredentialsProviderChain;

...

DefaultAWSCredentialsProviderChain credentialsProvider = DefaultAWSCredentialsProviderChain.create();
```

### Sync vs. Async Clients

The AWS SDK for Java provides both sync and async clients for each service. Sync clients execute operations in a blocking manner, whereas async clients execute operations asynchronously, returning a `CompletableFuture` that can be used to handle the result or perform further operations. Here's an example of creating an S3 client using both sync and async approaches:

```java
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.ListBucketsResponse;

...

// Sync client
S3Client syncClient = S3Client.builder()
                        .region(Region.US_EAST_1)
                        .credentialsProvider(credentialsProvider)
                        .build();

// Async client
S3AsyncClient asyncClient = S3AsyncClient.builder()
                            .region(Region.US_EAST_1)
                            .credentialsProvider(credentialsProvider)
                            .build();
```

### Sync Example - Listing S3 Buckets

Using the sync client, you can perform operations in a blocking manner. Here's an example of listing all S3 buckets using the sync approach:

```java
import software.amazon.awssdk.services.s3.model.ListBucketsResponse;
import software.amazon.awssdk.services.s3.model.S3Exception;

...

try {
    ListBucketsResponse response = syncClient.listBuckets();
    List<S3Bucket> buckets = response.buckets();

    for (S3Bucket bucket : buckets) {
        System.out.println("Bucket name: " + bucket.name());
    }
} catch (S3Exception e) {
    System.err.println("Error occurred: " + e.errorMessage());
}
```

### Async Example - Uploading a File to S3

Using the async client, you can perform operations asynchronously and handle the result using `CompletableFuture`. Here's an example of uploading a file to S3 using the async approach:

```java
import software.amazon.awssdk.services.s3.model.PutObjectRequest;
import software.amazon.awssdk.services.s3.model.PutObjectResponse;

...

String bucketName = "my-bucket";
String key = "path/to/file.txt";
String filePath = "/path/to/local/file.txt";

PutObjectRequest request = PutObjectRequest.builder()
                                .bucket(bucketName)
                                .key(key)
                                .build();
CompletableFuture<PutObjectResponse> future = asyncClient.putObject(request, Path.of(filePath));

future.whenComplete((response,

 exception) -> {
    if (exception != null) {
        System.err.println("Error occurred: " + exception.getMessage());
    } else {
        System.out.println("File uploaded successfully.");
    }
});
```

### Closing the Clients

After you have completed your operations, make sure to close the clients to release any allocated resources. Here's how you can close the sync and async clients:

```java
syncClient.close();
asyncClient.close();
```

## Conclusion

In this post, we explored the AWS SDK for Java and its usage for both sync and async programming styles. We covered setting up AWS credentials, creating sync and async clients, and performed examples for listing S3 buckets and uploading a file to S3. Make sure to refer to the official AWS SDK for Java documentation for more detailed information on each service and its available operations.

By leveraging the AWS SDK for Java, you can build powerful Java applications that integrate seamlessly with various AWS services.

References:
- [AWS SDK for Java Developer Guide](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/get-started.html)
- [AWS Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html)
- [Provide temporary credentials to the AWS SDK for Java](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html)