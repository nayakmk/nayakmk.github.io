---
layout: post
title: Connecting AWS EKS to S3 Using Spring Boot Application with Service Account
  Role
date: '2023-06-26 09:53:50 +0530'
categories: [AWS, EKS, S3, Spring Boot]
tags: [AWS, EKS, S3, Spring Boot, Service Account, IAM Role]
---

In this post, we will explore how to connect an AWS Elastic Kubernetes Service (EKS) cluster to Amazon S3 using a Spring Boot application. We will leverage the concept of service account roles in Kubernetes and IAM roles in AWS to enable secure communication between the EKS cluster and S3.

## Prerequisites

Before we begin, make sure you have the following prerequisites in place:

1. An AWS EKS cluster up and running.
2. An Amazon S3 bucket created.
3. A Spring Boot application with the necessary dependencies for working with AWS SDK.

## Step 1: Create IAM Role

First, we need to create an IAM role that allows our EKS cluster to access the S3 bucket. Follow these steps:

1. Go to the IAM service in the AWS Management Console.
2. Click on "Roles" in the left navigation panel.
3. Click on "Create role".
4. Select "EKS" as the trusted entity.
5. Choose the "Allows Amazon EKS to manage your clusters on your behalf" use case.
6. Attach the necessary policies that grant S3 access.
7. Provide a name for the role and create it.

## Step 2: Associate IAM Role with Service Account

Next, we need to associate the IAM role created in the previous step with the service account in our EKS cluster. Follow these steps:

1. Open the Kubernetes configuration file for your Spring Boot application.
2. Locate the `ServiceAccount` section and add the following annotation:

```yaml
annotations:
  eks.amazonaws.com/role-arn: arn:aws:iam::YOUR_AWS_ACCOUNT_ID:role/YOUR_IAM_ROLE_NAME
```

Replace `YOUR_AWS_ACCOUNT_ID` with your AWS account ID and `YOUR_IAM_ROLE_NAME` with the name of the IAM role created in Step 1.

3. Save the changes to the configuration file.

## Step 3: Configure AWS SDK in Spring Boot Application

Now, we need to configure the AWS SDK in our Spring Boot application to use the IAM role associated with the service account. Follow these steps:

1. Add the necessary dependencies to your Spring Boot application's `pom.xml` file:

```xml
<dependency>
    <groupId>software.amazon.awssdk</groupId>
    <artifactId>s3</artifactId>
</dependency>
<dependency>
    <groupId>software.amazon.awssdk</groupId>
    <artifactId>sts</artifactId>
</dependency>
```

2. Configure the AWS SDK with the IAM role in your application's configuration:

```java
@Configuration
public class AwsConfig {

    @Value("${aws.region}")
    private String awsRegion;

    @Bean
    public S3Client s3Client() {
        return S3Client.builder()
                .region(Region.of(awsRegion))
                .build();
    }
}
```

Make sure to set the `aws.region` property in your application's configuration file.

## Step 4: Access S3 from Spring Boot Application

With the AWS SDK configured, you can now access S3 from your Spring Boot application using the IAM role associated with the service account. Here's an example:

```java
@Service
public class S3Service {

    private final S3Client s3Client;

    @Autowired
    public S3Service(S3Client s3Client) {
        this.s3

Client = s3Client;
    }

    public void uploadFile(String bucketName, String key, File file) {
        PutObjectRequest request = PutObjectRequest.builder()
                .bucket(bucketName)
                .key(key)
                .build();

        s3Client.putObject(request, RequestBody.fromFile(file));
    }

    // Other methods for interacting with S3
}
```

In the above example, we inject the `S3Client` bean configured with the IAM role into the `S3Service` class and use it to perform S3 operations.

## Conclusion

By leveraging service account roles in EKS and IAM roles in AWS, we can securely connect an EKS cluster to S3 in a Spring Boot application. This allows seamless integration and access to S3 resources from within the EKS cluster.

Remember to follow AWS best practices for managing IAM roles, securing your EKS cluster, and implementing appropriate access controls for S3.

References:

- [AWS EKS Documentation](https://aws.amazon.com/eks/)
- [AWS IAM Documentation](https://aws.amazon.com/iam/)
- [Spring Boot Documentation](https://spring.io/projects/spring-boot)