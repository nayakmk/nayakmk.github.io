---
layout: post
title: 'Reactive Java: Understanding Flux Buffer and BufferTimeout Operators'
date: '2023-06-18 14:59:21 +0530'
categories: [Reactive Java, Flux]
tags: [Java, Reactive Programming, Web Development, Flux, Buffer]
---
## Introduction

The `buffer` operator in Flux allows you to group elements emitted by the source Flux into batches or buffers. Additionally, the `bufferTimeout` operator provides the capability to emit buffers based on both size and duration conditions.

In this post, we will explore the `buffer` and `bufferTimeout` operators in Flux and provide examples to demonstrate their usage.

## The `buffer` Operator

The `buffer` operator in Flux collects elements emitted by the source Flux into a container, such as a List or a Collection, and emits the container as a single element. This allows you to process elements in batches or groups.

The signature of the `buffer` operator is as follows:

```java
Flux<List<T>> buffer(int maxSize)
```

The `maxSize` parameter specifies the maximum number of elements to collect in each buffer. Once the buffer reaches this size, it is emitted as a List.

## The `bufferTimeout` Operator

The `bufferTimeout` operator in Flux combines both size and duration conditions to emit buffers. It collects elements emitted by the source Flux and emits a buffer when either the size condition or the duration condition is met.

The signature of the `bufferTimeout` operator is as follows:

```java
Flux<List<T>> bufferTimeout(int maxSize, Duration timespan)
```

The `maxSize` parameter specifies the maximum number of elements to collect in each buffer, and the `timespan` parameter specifies the maximum duration to wait before emitting the buffer, regardless of its size.

## Usage of `buffer` and `bufferTimeout` Operators

Let's explore some examples to understand the usage of the `buffer` and `bufferTimeout` operators in Flux.

### Example 1: Buffering Elements based on Size

```java
import reactor.core.publisher.Flux;

public class BufferSizeExample {

    public static void main(String[] args) {
        Flux.range(1, 10)
                .delayElements(Duration.ofMillis(500))
                .buffer(3)
                .subscribe(System.out::println);
    }
}
```

In this example, we have a Flux emitting elements from 1 to 10. We use the `buffer(3)` operator to collect the elements in groups of 3. The output will be a Flux of Lists, where each List represents a buffer containing 3 elements.

```output
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]
[10]
```

### Example 2: Buffering Elements based on Duration

```java
import reactor.core.publisher.Flux;

public class BufferSizeExample {

    public static void main(String[] args) {
        Flux.range(1, 10)
                .delayElements(Duration.ofMillis(500))
                .buffer(Duration.ofSeconds(5))
                .subscribe(System.out::println);
    }
}
```

In this example, we have a Flux emitting elements from 1 to 10. We use the `buffer(Duration.ofSeconds(5))` operator to collect the elements in duration of 5 seconds. The output will be a Flux of Lists, where each List represents a buffer containing elements which were emitted in last 5 seconds.

```output
[1, 2, 3, 4, 5, 6, 7, 8, 9]
[10]
```

### Example 3: Buffering Elements based on Size and Duration

```java
import reactor.core.publisher.Flux;
import java.time.Duration;

public class BufferTimeoutExample {

    public static void main(String[] args) {
        Flux.range(1,20)
                .delayElements(Duration.ofMillis(500))
                .bufferTimeout(3, Duration.ofSeconds(5))
                .subscribe(System.out::println);

        try {
            Thread.sleep(15000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

In this example, we use the `interval(Duration.ofSeconds(1))` Flux to emit elements every second. We use the `bufferTimeout(3, Duration.ofSeconds(5))` operator to collect the elements in groups of 3, or when the duration of 5 seconds elapses, whichever condition is met first. The output will be a Flux of Lists, where each List represents a buffer containing the collected elements.

```output
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]
[10, 11, 12]
[13, 14, 15]
[16, 17, 18]
[19, 20]
```

## Conclusion

The `buffer` and `bufferTimeout` operators in Flux provide convenient ways to group elements emitted by a Flux into batches or buffers based on different conditions. They offer flexibility in defining the size and duration criteria for emitting a buffer, allowing you to process data in chunks or based on specific time intervals.