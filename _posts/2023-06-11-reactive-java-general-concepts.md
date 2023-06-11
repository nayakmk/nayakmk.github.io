---
layout: post
title: 'Reactive Java: Concepts with Example'
date: '2023-06-11 13:11:19 +0530'
categories: [JAVA,REACTIVE]
tags: [reactive, java]
---

Reactive programming is a programming paradigm that focuses on asynchronous and event-driven programming to handle and process streams of data. In Java, several concepts are associated with reactive programming. Here are some key reactive concepts in Java:

### Publisher

A `Publisher` is a source of data in the reactive programming model. It emits a stream of items, often asynchronously, to one or more subscribers. The `Publisher` interface is part of the Reactive Streams specification and is a fundamental building block for reactive programming in Java.

A `Publisher` emits a stream of events or values over time. Here are some examples of reactive Java `Publisher` implementations:

#### Flux

The `Flux` class from the Reactor library is a reactive `Publisher` that emits zero or more elements over time. It can emit elements synchronously or asynchronously.

```java
import reactor.core.publisher.Flux;

public class FluxExample {
    public static void main(String[] args) {
        Flux<Integer> numbers = Flux.just(1, 2, 3, 4, 5);

        numbers.subscribe(System.out::println);
    }
}
```

#### Mono

The `Mono` class from the Reactor library is a reactive `Publisher` that emits at most one element over time. It represents a single-value or empty sequence.

```java
import reactor.core.publisher.Mono;

public class MonoExample {
    public static void main(String[] args) {
        Mono<String> greeting = Mono.just("Hello, World!");

        greeting.subscribe(System.out::println);
    }
}
```

#### Custom Publisher

You can also create your custom `Publisher` by implementing the `Publisher` interface. Here's an example of a simple custom `Publisher`:

```java
import java.util.concurrent.Flow.*;

public class CustomPublisherExample implements Publisher<String> {

    @Override
    public void subscribe(Subscriber<? super String> subscriber) {
        subscriber.onSubscribe(new Subscription() {
            @Override
            public void request(long n) {
                subscriber.onNext("Hello");
                subscriber.onNext("World");
                subscriber.onComplete();
            }

            @Override
            public void cancel() {
                // Handle cancellation
            }
        });
    }

    public static void main(String[] args) {
        CustomPublisherExample publisher = new CustomPublisherExample();
        publisher.subscribe(new Subscriber<String>() {
            @Override
            public void onSubscribe(Subscription subscription) {
                subscription.request(Long.MAX_VALUE);
            }

            @Override
            public void onNext(String item) {
                System.out.println(item);
            }

            @Override
            public void onError(Throwable throwable) {
                // Handle error
            }

            @Override
            public void onComplete() {
                System.out.println("Completed");
            }
        });
    }
}
```

In this example, the custom `Publisher` emits "Hello" and "World" as elements and completes the stream.

### Subscriber

A `Subscriber` is a consumer of data in the reactive programming model. It subscribes to a `Publisher` to receive items emitted by the `Publisher`. The `Subscriber` interface defines methods for handling emitted items, errors, and completion signals.

A `Subscriber` is responsible for consuming and processing the data emitted by a `Publisher`. Here are some examples of reactive Java `Subscriber` implementations:

#### BaseSubscriber

The `BaseSubscriber` class from the Reactor library provides a base implementation of the `Subscriber` interface with additional methods for handling backpressure and other stream control.

```java
import reactor.core.publisher.BaseSubscriber;

public class BaseSubscriberExample {
    public static void main(String[] args) {
        CustomSubscriber subscriber = new CustomSubscriber();

        Flux.range(1, 10)
            .subscribe(subscriber);
    }
}

class CustomSubscriber extends BaseSubscriber<Integer> {
    @Override
    protected void hookOnNext(Integer value) {
        System.out.println("Received: " + value);
        if (value == 5) {
            cancel(); // Cancels the subscription after receiving the value 5
        }
    }
}
```

In this example, we create a custom `Subscriber` by extending the `BaseSubscriber` class. We override the `hookOnNext` method to process each emitted value. We also cancel the subscription when the value reaches 5 by calling the `cancel` method.

#### LambdaSubscriber

You can also use a lambda expression to define a `Subscriber` inline, without creating a custom class.

```java
import java.util.concurrent.Flow.*;

public class LambdaSubscriberExample {
    public static void main(String[] args) {
        Subscriber<Integer> subscriber = new LambdaSubscriber<>(
            System.out::println,
            Throwable::printStackTrace,
            () -> System.out.println("Completed")
        );

        Flux.range(1, 10)
            .subscribe(subscriber);
    }
}
```

In this example, we use the `LambdaSubscriber` class, which is a helper class provided by the Java `Flow` API. We pass lambda expressions for the `onNext`, `onError`, and `onComplete` methods to define the behavior of the `Subscriber`.

### Subscription

The `Subscription` represents the connection between a `Publisher` and a `Subscriber`. It provides methods for requesting items from the `Publisher`, canceling the subscription, and managing backpressure. This allows the `Subscriber` to control the flow of data from the `Publisher`. Here are some examples of reactive Java `Subscription` usage:

#### Requesting Elements

The `request` method of the `Subscription` interface is used by the `Subscriber` to request a specific number of elements from the `Publisher`.

```java
import java.util.concurrent.Flow.*;

public class SubscriptionExample {
    public static void main(String[] args) {
        Subscriber<Integer> subscriber = new CustomSubscriber();

        Flux.range(1, 10)
            .subscribe(subscriber);
    }
}

class CustomSubscriber implements Subscriber<Integer> {
    private Subscription subscription;

    @Override
    public void onSubscribe(Subscription subscription) {
        this.subscription = subscription;
        subscription.request(3); // Requesting three elements initially
    }

    @Override
    public void onNext(Integer item) {
        System.out.println("Received: " + item);
        if (item == 3) {
            subscription.request(2); // Requesting two more elements after receiving the third element
        }
    }

    // Other methods
}
```

In this example, the `onSubscribe` method of the `Subscriber` receives the `Subscription` object. We store the `Subscription` instance for later use. Then, we call `subscription.request(3)` to request three initial elements from the `Publisher`. In the `onNext` method, we check if the received item is 3, and then we call `subscription.request(2)` to request two more elements.

#### Cancelling Subscription

The `cancel` method of the `Subscription` interface is used by the `Subscriber` to cancel the subscription and stop receiving further elements.

```java
import java.util.concurrent.Flow.*;

public class SubscriptionExample {
    public static void main(String[] args) {
        Subscriber<Integer> subscriber = new CustomSubscriber();

        Flux.range(1, 10)
            .subscribe(subscriber);
    }
}

class CustomSubscriber implements Subscriber<Integer> {
    private Subscription subscription;

    @Override
    public void onSubscribe(Subscription subscription) {
        this.subscription = subscription;
        subscription.request(Long.MAX_VALUE); // Requesting all elements
    }

    @Override
    public void onNext(Integer item) {
        System.out.println("Received: " + item);
        if (item == 5) {
            subscription.cancel(); // Cancelling subscription after receiving the fifth element
        }
    }

    // Other methods
}
```

In this example, we call `subscription.request(Long.MAX_VALUE)` in the `onSubscribe` method to request all elements from the `Publisher`. In the `onNext` method, we check if the received item is 5, and then we call `subscription.cancel()` to cancel the subscription.

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