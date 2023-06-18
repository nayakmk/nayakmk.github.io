---
layout: post
title: 'Reactive Java: Parallel Processing with PublishOn and SubscribeOn'
date: '2023-06-17 12:49:25 +0530'
categories: [Reactive Java]
tags: [Java, Reactive Programming, Parallel Processing]
---
## Introduction

Parallel processing is a powerful technique in Reactive Java that allows you to achieve concurrency and parallelism in your reactive streams. Reactive Java provides two key operators, `publishOn` and `subscribeOn`, which enable you to control the execution context and parallelism of your reactive streams. In this post, we will explore the usage of `publishOn` and `subscribeOn` operators and provide examples to illustrate their usages.

## PublishOn Operator

The `publishOn` operator in Reactive Java is used to specify the scheduler on which the downstream operators will execute. It controls the execution context of subsequent operators in the stream, allowing you to achieve parallelism and control the threading model.

Here's an example of using the `publishOn` operator:

```java
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

Flux.range(1, 10)
    .map(i -> {
        System.out.println("Map : " + i + " Thread: " + Thread.currentThread().getName());
        return i * 2;
    })
    .publishOn(Schedulers.parallel())
    .map(i -> {
        System.out.println("Map : " + i + " Thread: " + Thread.currentThread().getName());
        return i * 3;
    })
    .subscribeOn(Schedulers.single())
    .subscribe();
```

In this example, the `publishOn(Schedulers.parallel())` operator is used to specify the parallel scheduler for the downstream operators. The `map` operator after `publishOn` will be executed on the parallel scheduler, enabling parallel processing. The output may vary depending on the execution order of threads:

```
Map : 1 Thread: single-1
Map : 2 Thread: single-1
Map : 3 Thread: single-1
Map : 4 Thread: single-1
Map : 5 Thread: single-1
Map : 6 Thread: single-1
Map : 7 Thread: single-1
Map : 8 Thread: single-1
Map : 9 Thread: single-1
Map : 10 Thread: single-1
Map : 2 Thread: parallel-1
Map : 4 Thread: parallel-1
Map : 6 Thread: parallel-1
Map : 8 Thread: parallel-1
Map : 10 Thread: parallel-1
Map : 12 Thread: parallel-1
Map : 14 Thread: parallel-1
Map : 16 Thread: parallel-1
Map : 18 Thread: parallel-1
Map : 20 Thread: parallel-1
```

## SubscribeOn Operator

The `subscribeOn` operator in Reactive Java is used to specify the scheduler on which the source emits elements and performs the initial subscription. It controls the execution context of the entire reactive stream.

Here's an example of using the `subscribeOn` operator:

```java
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

Flux.range(1, 10)
    .subscribeOn(Schedulers.parallel())
    .map(i -> {
        System.out.println("Map : " + i + " Thread: " + Thread.currentThread().getName());
        return i * 2;
    })
    .subscribe();
```

In this example, the `subscribeOn(Schedulers.parallel())` operator is used to specify the parallel scheduler for the source stream. The `map` operator will be executed on the parallel scheduler as well, enabling parallel processing. The output may vary depending on the execution order of threads:

```
Map : 1 Thread: parallel-1
Map : 2 Thread: parallel-1
Map : 3 Thread: parallel-1
Map : 4 Thread: parallel-1
Map : 5 Thread: parallel-1
Map : 6 Thread: parallel-1
Map : 7 Thread: parallel-1
Map : 8 Thread: parallel-1
Map : 9 Thread: parallel-1
Map : 10 Thread: parallel-1
```

## Usage and Benefits

The `publishOn` and `subscribeOn` operators in Reactive Java offer several benefits:

1. **Parallelism**: By using these operators, you can achieve parallel processing and leverage the power of multiple threads or processors to execute your reactive streams concurrently, leading to improved performance.

2. **Control over Execution Context**: With `publishOn` and `subscribeOn`, you have fine-grained control over the execution context and threading model of your reactive streams. You can specify the desired schedulers and parallelize specific parts of your stream.

3. **Efficient Resource Utilization**: By utilizing parallel schedulers, you can optimize the utilization of system resources and achieve better efficiency in processing your reactive streams.

4. **Scalability**: Reactive Java's parallel processing capabilities allow your application to scale and handle larger workloads effectively, ensuring responsiveness and efficiency.

## Conclusion

The `publishOn` and `subscribeOn` operators in Reactive Java provide powerful mechanisms for achieving parallel processing and controlling the execution context of your reactive streams. By utilizing these operators, you can leverage parallel schedulers, achieve concurrency, and optimize resource utilization. This enables your reactive applications to handle larger workloads efficiently and improve overall performance.