---
layout: post
title: 'Reactive Java: Deferred Execution with Reactive Java''s defer Operator'
date: '2023-06-17 10:47:00 +0530'
categories: [Reactive Java]
tags: [Java, Reactive Programming, Defer]
---
## Introduction

In Reactive Java programming, the `defer` operator is a powerful tool that allows for deferred execution of code within a reactive stream. It defers the creation of the actual `Flux` or `Mono` until a subscriber subscribes to it. In this post, we will explore the usage of the `defer` operator in Reactive Java with examples to illustrate its benefits.

## Understanding the Defer Operator

The `defer` operator in Reactive Java is used to defer the creation of a `Flux` or `Mono` until the moment of subscription. It accepts a `Supplier` that is invoked for each subscriber, creating a new instance of the `Flux` or `Mono` on-demand. This allows for the execution of custom logic or calculations that need to be performed each time a subscriber subscribes.

## Example 1: Defer Execution of a Flux

```java
import reactor.core.publisher.Flux;

Flux<Integer> deferredFlux = Flux.defer(() -> {
    System.out.println("Creating Flux");
    return Flux.range(1, 5);
});

System.out.println("Before subscribing");

deferredFlux.subscribe(number -> System.out.println("Subscriber 1: " + number));
deferredFlux.subscribe(number -> System.out.println("Subscriber 2: " + number));

System.out.println("After subscribing");
```

In this example, we use the `defer` operator to create a `Flux` that emits numbers from 1 to 5. However, the actual creation of the `Flux` and the emission of values are deferred until a subscriber subscribes to it.

When we run the above code, the output will be:

```
Before subscribing
Creating Flux
Subscriber 1: 1
Subscriber 1: 2
Subscriber 1: 3
Subscriber 1: 4
Subscriber 1: 5
Creating Flux
Subscriber 2: 1
Subscriber 2: 2
Subscriber 2: 3
Subscriber 2: 4
Subscriber 2: 5
After subscribing
```

As we can see, the creation of the `Flux` and the emission of values occur only when subscribers subscribe to it. Each subscriber receives its own sequence of numbers emitted by the `Flux`.

## Example 2: Defer Execution of a Mono

```java
import reactor.core.publisher.Mono;

Mono<String> deferredMono = Mono.defer(() -> {
    System.out.println("Creating Mono");
    return Mono.just("Hello, World!");
});

System.out.println("Before subscribing");

deferredMono.subscribe(message -> System.out.println("Subscriber 1: " + message));
deferredMono.subscribe(message -> System.out.println("Subscriber 2: " + message));

System.out.println("After subscribing");
```

In this example, we use the `defer` operator to create a `Mono` that emits a single string message, "Hello, World!". Similar to the previous example, the creation of the `Mono` and the emission of the value are deferred until a subscriber subscribes to it.

When we run the above code, the output will be:

```
Before subscribing
Creating Mono
Subscriber 1: Hello, World!
Creating Mono
Subscriber 2: Hello, World!
After subscribing
```

As expected, the creation of the `Mono` and the emission of the value occur only when subscribers subscribe to it. Each subscriber receives the same string message emitted by

 the `Mono`.

## Usage and Benefits

The `defer` operator in Reactive Java offers several benefits:

1. **Lazy Evaluation**: The `defer` operator allows for lazy evaluation of the `Flux` or `Mono`, ensuring that expensive computations or operations are performed only when necessary.

2. **Dynamic Behavior**: By deferring the creation of the `Flux` or `Mono`, you can incorporate dynamic logic or calculations based on the subscriber's context.

3. **Reusability**: Since a new instance of the `Flux` or `Mono` is created for each subscription, you can reuse the `defer` operator to create multiple reactive streams with different behavior.

4. **Consistency**: The `defer` operator ensures that each subscriber receives its own independent sequence of values, maintaining the consistency of the reactive stream.

## Conclusion

The `defer` operator in Reactive Java provides a convenient way to defer the execution and creation of `Flux` or `Mono` until the moment of subscription. It allows for lazy evaluation, dynamic behavior, and consistent results across subscribers.

By understanding the `defer` operator and its usage, you can leverage its benefits to incorporate deferred execution and create reactive streams with customized behavior in your Reactive Java applications.
