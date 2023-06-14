---
layout: post
title: 'Reactive Java: Mono fromSupplier vs fromCallable'
date: '2023-06-13 23:49:03 +0530'
categories: [JAVA,REACTIVE]
tags: [reactive, mono, reactor]
---

The methods `Mono.fromSupplier()` and `Mono.fromCallable()` are both used in the Reactor library, which is an implementation of the Reactive Streams specification for Java. These methods are used to create `Mono` instances, which represent reactive streams that emit at most one item.

The main difference between `Mono.fromSupplier()` and `Mono.fromCallable()` lies in the types of functional interfaces they accept.

### `Mono.fromSupplier()`

This method expects a `Supplier<T>` functional interface as its argument. The `Supplier` interface has a single method `get()` that takes no arguments and returns a value of type `T`. When the `Mono` instance created by `Mono.fromSupplier()` is subscribed to, the `get()` method of the supplied `Supplier` will be invoked to produce the value emitted by the `Mono`. The `Supplier` allows lazy evaluation, meaning the value is generated only when requested by a subscriber.

Here's an example of `Mono.fromSupplier()`:

```java
Mono<String> mono = Mono.fromSupplier(() -> "Hello, world!");
```

### `Mono.fromCallable()`

This method expects a `Callable<T>` functional interface as its argument. The `Callable` interface also has a single method `call()` that takes no arguments and returns a value of type `T`. When the `Mono` instance created by `Mono.fromCallable()` is subscribed to, the `call()` method of the supplied `Callable` will be invoked to produce the value emitted by the `Mono`. The `Callable` is similar to `Supplier` but allows throwing checked exceptions.

Here's an example of `Mono.fromCallable()`:

```java
Mono<String> mono = Mono.fromCallable(() -> {
    // Perform some expensive or potentially error-prone computation
    return "Hello, world!";
});
```

In summary, the main difference between `Mono.fromSupplier()` and `Mono.fromCallable()` is the type of functional interface they expect. `fromSupplier()` takes a `Supplier` without checked exceptions, whereas `fromCallable()` takes a `Callable` that can throw checked exceptions. Both methods allow lazily generating the value when the `Mono` is subscribed to, but `fromCallable()` provides the flexibility of handling exceptions within the supplied callable.