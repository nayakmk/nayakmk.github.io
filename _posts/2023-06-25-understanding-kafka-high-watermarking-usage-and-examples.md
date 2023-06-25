---
layout: post
title: 'Understanding Kafka High Watermarking: Usage and Examples'
date: '2023-06-25 23:07:53 +0530'
categories: [Messaging, Apache Kafka]
tags: [Kafka, High Watermarking, Messaging, Event Streaming]
---

In Apache Kafka, high watermarking is an essential concept that plays a crucial role in tracking the progress of message consumption by consumers within a Kafka cluster. Understanding high watermarking is key to ensuring reliable and efficient message processing. In this post, we'll delve into the concept of Kafka high watermarking, explore its usage, and provide examples to illustrate its significance. Let's get started!

## What is Kafka High Watermarking?

Kafka high watermarking, also known as the "high watermark" or "commit point," represents the offset of the latest message that has been fully replicated across all the in-sync replicas (ISRs) of a Kafka partition. In simpler terms, it is the highest offset value that has been acknowledged and safely stored by all replicas in a partition.

The high watermark acts as a progress marker that indicates the point up to which messages have been successfully replicated and committed. It ensures that consumers can reliably consume messages that are beyond the high watermark, as they are guaranteed to be durable and available.

## Usage and Significance of High Watermarking

### Consumer Offset Management

The high watermark is critical for managing consumer offsets. Kafka consumers use the high watermark as a reference point to determine the latest committed offset. By tracking the high watermark, consumers can ensure that they consume messages that are safely stored and replicated.

### Data Durability and Reliability

The high watermark is a key factor in ensuring the durability and reliability of data in Kafka. Messages that are below the high watermark may still be in the process of replication or may not be fully guaranteed for availability. On the other hand, messages beyond the high watermark are considered to be durable and available for consumption.

### Rebalancing and Leader Election

During consumer group rebalancing or leader election scenarios, the high watermark helps in determining the progress and readiness of replicas. It ensures that new leaders and consumers do not consume messages that are not fully replicated or committed.

## High Watermark Example

Let's consider a simple example to illustrate the concept of high watermarking. Suppose we have a Kafka topic with three partitions, and the current high watermark for each partition is as follows:

- Partition 0: High Watermark = 100
- Partition 1: High Watermark = 120
- Partition 2: High Watermark = 95

Based on the high watermark values, consumers can safely consume messages starting from the offset indicated by the high watermark for each partition. In this case, consumers can consume messages from offset 101 for Partition 0, offset 121 for Partition 1, and offset 96 for Partition 2.

## Conclusion

Kafka high watermarking is a crucial concept that ensures the reliability and durability of data in a Kafka cluster. By tracking the high watermark, consumers can consume messages that are safely replicated and committed. In this post, we explored the usage and significance of high watermarking, discussing its role in consumer offset management, data durability, and rebalancing scenarios. We also provided an example to illustrate the concept. Understanding high watermarking is essential for building robust and efficient Kafka-based messaging systems.