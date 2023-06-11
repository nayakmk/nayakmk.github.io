---
layout: post
title: 'Reactive Java: Flux - doOnNext vs flatMap'
date: '2023-06-11 13:55:55 +0530'
---

## Reactive Java: Flux - doOnNext vs flatMap

`doOnNext` and `flatMap` are two different operators in Reactive Java, specifically in the context of the Project Reactor library. Here's an explanation of each operator and their differences:

### `doOnNext` Operator

The `doOnNext` operator is a side-effect operator in Reactive Java. It allows you to perform a specific action or operation for each emitted item in the stream without modifying the stream itself. It is primarily used for logging, monitoring, or triggering side effects.

**Example**:
```java
Flux.range(1, 5)
    .doOnNext(item -> System.out.println("Received: " + item))
    .subscribe();
```
In this example, the `doOnNext` operator logs each item before it is passed down the stream.

### `flatMap` Operator

The `flatMap` operator is a transformation operator in Reactive Java. It is used to transform each item emitted by a publisher into another publisher or a sequence of publishers, allowing for the creation of complex data flows and asynchronous operations.

**Example**:
```java
Flux.range(1, 3)
    .flatMap(item -> Flux.just(item, item * 2))
    .subscribe(System.out::println);
```
In this example, the `flatMap` operator transforms each item into a sequence of two items (the original item and its double). The resulting stream contains all the transformed items.

**Key Differences**:
- **Purpose**: `doOnNext` is used for side effects and doesn't modify the stream, while `flatMap` is used for transforming the stream by mapping each item to a new publisher or sequence of publishers.
- **Output**: `doOnNext` doesn't modify the items emitted by the stream, whereas `flatMap` can transform each item and potentially expand the stream with multiple items or different types.
- **Asynchronicity**: `flatMap` can handle asynchronous operations by creating new publishers for each item, allowing concurrent processing. `doOnNext` does not provide concurrency control or handle asynchronicity.

In summary, `doOnNext` is used for side effects and logging, while `flatMap` is used for transforming the stream by mapping each item to one or more publishers. They serve different purposes in Reactive Java and are used in different scenarios to achieve specific behaviors.