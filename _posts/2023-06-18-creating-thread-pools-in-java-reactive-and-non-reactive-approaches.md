---
layout: post
title: 'Creating Thread Pools in Java: Reactive and Non-Reactive Approaches'
date: '2023-06-18 16:16:46 +0530'
categories: [Java, Multithreading]
tags: [Java, Multithreading, Concurrency, Reactive Programming]
---

## Introduction

Thread pools are a fundamental concept in Java for managing and executing tasks concurrently. They provide a pool of reusable threads that can be used to execute tasks efficiently. In this post, we will explore various ways to create thread pools in Java, covering both the reactive and non-reactive approaches. Additionally, we will discuss specific thread pool configurations for compute-bound and I/O-bound tasks. We will provide examples to illustrate the usage of each approach.

## Non-Reactive Thread Pools

### Using `ExecutorService` for Compute-Bound Tasks

The non-reactive approach to creating thread pools in Java involves using the `ExecutorService` class from the `java.util.concurrent` package. For compute-bound tasks, where the tasks involve CPU-intensive operations, a fixed-size thread pool is commonly used.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        // Create a fixed-size thread pool for compute-bound tasks
        ExecutorService executor = Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());

        // Execute compute-bound tasks using the thread pool
        for (int i = 0; i < 10; i++) {
            final int taskId = i;
            executor.execute(() -> {
                // Perform compute-bound task logic
                System.out.println("Compute-bound task " + taskId + " executed by thread: " + Thread.currentThread().getName());
            });
        }

        // Shutdown the thread pool
        executor.shutdown();
    }
}
```

In this example, we create a fixed-size thread pool using the `Executors.newFixedThreadPool()` method, passing the number of available processors as the pool size. This configuration ensures that the thread pool has enough threads to fully utilize the available CPU cores. We then submit compute-bound tasks to the thread pool using the `execute()` method. Each task is executed by one of the threads from the pool.

### Using `ExecutorService` with Cached Thread Pool for I/O-Bound Tasks

For I/O-bound tasks, where the tasks involve blocking I/O operations, a cached thread pool is commonly used. This type of thread pool dynamically adjusts the number of threads based on the workload.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        // Create a cached thread pool for I/O-bound tasks
        ExecutorService executor = Executors.newCachedThreadPool();

        // Execute I/O-bound tasks using the thread pool
        for (int i = 0; i < 10; i++) {
            final int taskId = i;
            executor.execute(() -> {
                // Perform I/O-bound task logic
                System.out.println("I/O-bound task " + taskId + " executed by thread: " + Thread.currentThread().getName());
            });
        }

        // Shutdown the thread pool
        executor.shutdown();
    }
}
```

In this example, we create a cached thread pool using the `Executors.newCachedThreadPool()` method. This type of thread pool is suitable for I/O-bound tasks as it can dynamically adjust the number of threads based on the workload. We submit I/O-bound tasks to the thread pool using the `execute()` method, and each task is executed by one of the threads from the pool.

## Reactive Thread

 Pools

### Using Project Reactor for Compute-Bound Tasks

The reactive approach to creating thread pools in Java involves using the Project Reactor library, which provides abstractions for reactive programming. For compute-bound tasks, we can configure a fixed-size thread pool using the `Schedulers` class.

```java
import reactor.core.scheduler.Schedulers;

public class Main {
    public static void main(String[] args) {
        // Execute compute-bound tasks on a fixed-size thread pool
        for (int i = 0; i < 10; i++) {
            final int taskId = i;
            Schedulers.newParallel("ComputePool", Runtime.getRuntime().availableProcessors()).schedule(() -> {
                // Perform compute-bound task logic
                System.out.println("Compute-bound task " + taskId + " executed by thread: " + Thread.currentThread().getName());
            });
        }
    }
}
```

In this example, we use the `Schedulers.newParallel()` method from Project Reactor to create a fixed-size thread pool for compute-bound tasks. We pass the pool name and the number of available processors as arguments to the method. We then use the `schedule()` method to submit compute-bound tasks to the thread pool. Each task is executed asynchronously by one of the threads from the pool.

### Using Project Reactor for I/O-Bound Tasks

For I/O-bound tasks, Project Reactor provides a dedicated thread pool called the "elastic" thread pool. This thread pool can dynamically adjust the number of threads based on the workload.

```java
import reactor.core.scheduler.Schedulers;

public class Main {
    public static void main(String[] args) {
        // Execute I/O-bound tasks asynchronously on the elastic thread pool
        for (int i = 0; i < 10; i++) {
            final int taskId = i;
            Schedulers.newElastic("IOPool").schedule(() -> {
                // Perform I/O-bound task logic
                System.out.println("I/O-bound task " + taskId + " executed by thread: " + Thread.currentThread().getName());
            });
        }
    }
}
```

In this example, we use the `Schedulers.newElastic()` method from Project Reactor to create an elastic thread pool for I/O-bound tasks. We pass the pool name as an argument to the method. We then use the `schedule()` method to submit I/O-bound tasks to the thread pool. Each task is executed asynchronously by one of the threads from the pool.

## Conclusion

Creating thread pools in Java is crucial for managing concurrent tasks effectively. In this post, we explored various ways to create thread pools, both in reactive and non-reactive contexts. For compute-bound tasks, a fixed-size thread pool is commonly used, while for I/O-bound tasks, a cached or elastic thread pool is more suitable. By choosing the appropriate thread pool configuration, you can optimize the execution of your tasks based on their characteristics. Understanding these different approaches will help you design efficient and scalable Java applications that make the best use of concurrency.