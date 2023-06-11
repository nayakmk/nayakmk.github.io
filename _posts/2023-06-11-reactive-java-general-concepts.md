---
layout: post
title: 'Reactive Java: Concepts with Example'
date: '2023-06-11 13:11:19 +0530'
---

## Reactive Java Concepts with Example

Reactive programming is a programming paradigm that focuses on asynchronous and event-driven programming to handle and process streams of data. In Java, several concepts are associated with reactive programming. Here are some key reactive concepts in Java:

### Publisher

A `Publisher` is a source of data in the reactive programming model. It emits a stream of items, often asynchronously, to one or more subscribers. The `Publisher` interface is part of the Reactive Streams specification and is a fundamental building block for reactive programming in Java.

### Subscriber

A `Subscriber` is a consumer of data in the reactive programming model. It subscribes to a `Publisher` to receive items emitted by the `Publisher`. The `Subscriber` interface defines methods for handling emitted items, errors, and completion signals.

### Subscription

The `Subscription` represents the connection between a `Publisher` and a `Subscriber`. It provides methods for requesting items from the `Publisher`, canceling the subscription, and managing backpressure.

### Operator

Operators in reactive programming allow the transformation, filtering, and manipulation of the data stream. Operators can be applied to a `Publisher` to create a new `Publisher` that modifies the stream in some way. Common operators include `map`, `filter`, `flatMap`, `merge`, `concat`, and `reduce`.

### Backpressure

Backpressure is a mechanism for handling the flow of data when there is a mismatch in processing speeds between the `Publisher` and the `Subscriber`. It allows the `Subscriber` to control the rate at which it receives items from the `Publisher` to prevent overwhelm and ensure efficient resource utilization.

```java
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

public class ReactiveJavaBackpressureExample {
    public static void main(String[] args) {
        // Create a fast producer emitting integers from 1 to 1000
        Flux<Integer> producer = Flux.range(1, 1000)
                .subscribeOn(Schedulers.parallel());

        // Slow down the consumer by introducing a delay
        Flux<Integer> slowConsumer = producer
                .delayElements(Duration.ofMillis(100))
                .doOnNext(i -> {
                    // Simulate slow processing
                    try {
                        Thread.sleep(200);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                })
                .doOnRequest(n -> System.out.println("Consumer requested: " + n))
                .doOnComplete(() -> System.out.println("Consumer completed"));

        // Subscribe the slow consumer with backpressure
        slowConsumer.subscribe(new BaseSubscriber<Integer>() {
            @Override
            protected void hookOnSubscribe(Subscription subscription) {
                // Request only 10 items initially
                request(10);
            }

            @Override
            protected void hookOnNext(Integer value) {
                // Process the item
                System.out.println("Consumer processing: " + value);
                // Request the next item
                request(1);
            }
        });

        // Wait for the reactive streams to complete
        try {
            Thread.sleep(10000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

In this example, we create a fast producer `Flux` that emits integers from 1 to 1000 on the parallel scheduler. We introduce a delay of 100 milliseconds between each emitted item.

We then create a slow consumer `Flux` by subscribing to the producer. The slow consumer simulates slow processing by introducing a delay of 200 milliseconds for each item. It also logs the number of requested items and completion events.

To handle backpressure, we use a custom `BaseSubscriber` and override the `hookOnSubscribe` and `hookOnNext` methods. In `hookOnSubscribe`, we initially request only 10 items. In `hookOnNext`, we process the item, print its value, and request the next item.

By applying backpressure with controlled item requests, the consumer can manage the flow of items from the producer. This ensures that the consumer can handle items at its own pace, preventing it from being overwhelmed by a fast producer.

The example demonstrates how backpressure allows the consumer to regulate the amount of data it can handle, providing a balanced and controlled flow of data through the reactive pipeline.

### Scheduler

A scheduler provides a context for executing reactive code. It manages threads or thread pools and determines the execution environment for tasks such as emitting items, applying operators, and handling subscriptions. Schedulers help control concurrency and ensure the proper execution of reactive streams.

```java
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

public class ReactiveJavaSchedulersExample {
    public static void main(String[] args) {
        // Create a Flux emitting integers from 1 to 5
        Flux<Integer> flux = Flux.range(1, 5);

        // Execute the reactive code on the immediate scheduler
        flux
                .map(i -> {
                    System.out.println("Map 1: " + Thread.currentThread().getName() + " - " + i);
                    return i * 2;
                })
                .subscribeOn(Schedulers.immediate())
                .subscribe( i -> System.out.println("Scheduler Map 2: " + Thread.currentThread().getName() + " - " + i));

        // Execute the reactive code on the parallel scheduler
        flux
                .map(i -> {
                    System.out.println("Map 2: " + Thread.currentThread().getName() + " - " + i);
                    return i * 2;
                })
                .subscribeOn(Schedulers.parallel())
                .subscribe( i -> System.out.println("Scheduler Map 2: " + Thread.currentThread().getName() + " - " + i));

        // Wait for the reactive code to complete
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

In this example, we create a `Flux` that emits integers from 1 to 5. We then perform two transformations using the `map` operator. Each `map` operation includes a print statement to show the current thread executing the operation.

We use different schedulers for each `Flux` to demonstrate the behavior of schedulers:

- The first `Flux` is executed on the `Schedulers.immediate()` scheduler, which runs the code on the calling thread. The output shows that the `map` operation is executed on the main thread.
- The second `Flux` is executed on the `Schedulers.parallel()` scheduler, which provides a pool of worker threads. The output shows that the `map` operation is executed on different parallel threads.

```java
Map 1: main - 1
Scheduler Map 2: main - 2
Map 1: main - 2
Scheduler Map 2: main - 4
Map 1: main - 3
Scheduler Map 2: main - 6
Map 1: main - 4
Scheduler Map 2: main - 8
Map 1: main - 5
Scheduler Map 2: main - 10
Map 2: parallel-1 - 1
Scheduler Map 2: parallel-1 - 2
Map 2: parallel-1 - 2
Scheduler Map 2: parallel-1 - 4
Map 2: parallel-1 - 3
Scheduler Map 2: parallel-1 - 6
Map 2: parallel-1 - 4
Scheduler Map 2: parallel-1 - 8
Map 2: parallel-1 - 5
Scheduler Map 2: parallel-1 - 10
```



By using **schedulers**, you can control the execution context of your reactive code, allowing it to run on different threads, thread pools, or even specific execution contexts. This enables you to manage concurrency, parallelism, and resource utilization effectively in your reactive Java applications.

### Flux/Mono

Flux and Mono are types provided by reactive libraries like Project Reactor and RxJava. Flux represents a stream of zero or more items, while Mono represents a stream that emits at most one item or an error signal. These types provide a rich set of operators and methods for working with reactive streams.

### Hot and Cold Publishers

Hot publishers emit data regardless of whether there are active subscribers, whereas cold publishers emit data only when there is an active subscriber. Hot publishers are useful for broadcasting events, while cold publishers are often used for generating data on-demand.