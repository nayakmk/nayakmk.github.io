---
layout: post
title: 'Reactive Java: Flux Sink vs Synchronous Sink'
date: '2023-06-13 23:55:06 +0530'
categories: [JAVA,REACTIVE]
tags: [reactive, reactor, flux]
---

In Reactor, the terms "Flux Sink" and "Synchronous Sink" refer to different types of sinks that can be used to emit elements in reactive streams.

### Flux Sink 

`FluxSink` is a sink that allows you to manually emit elements in a Flux stream. It provides methods to emit elements, handle backpressure, and complete or error the stream. You can create a `FluxSink` using the `Flux.create()` method, which takes a consumer as a parameter. The consumer receives the `FluxSink` instance, allowing you to emit elements and control the stream.

Here's an example of using `FluxSink`:

```java
Flux<String> flux = Flux.create(sink -> {
    sink.next("element1");
    sink.next("element2");
    sink.complete();
});
```

In the above example, the `FluxSink` is used to manually emit two elements ("element1" and "element2") and then complete the stream.

### Synchronous Sink

`SynchronousSink` is an interface that represents a sink for synchronous emission of elements in a Flux stream. It is typically used within the `Flux.generate()` method, which allows you to create a stream with custom logic for emitting elements. The `SynchronousSink` provides methods to emit elements, handle backpressure, and terminate the stream.

Here's an example of using `SynchronousSink` with `Flux.generate()`:

```java
Flux<String> flux = Flux.generate(
    () -> 0,
    (state, sink) -> {
        sink.next("element" + state);
        if (state == 2) {
            sink.complete();
        }
        return state + 1;
    }
);
```

In the above example, the `SynchronousSink` is used within the `Flux.generate()` method to emit elements ("element0", "element1", and "element2") synchronously. The generator function takes a state and a `SynchronousSink` instance as arguments. It emits the current element based on the state and updates the state for the next emission. When the state reaches 2, it completes the stream.

Another example of using `SynchronousSink` with `Flux.generate()`:

```java
Flux<Integer> flux = Flux.generate(
    sink -> {
        sink.next(0);
    },
    (state, sink) -> {
        int nextState = state + 1;
        if (nextState <= 9) {
            sink.next(nextState);
        } else {
            sink.complete();
        }
        return nextState;
    }
);

flux.subscribe(System.out::println);
```

Please note that the `SynchronousSink` is used specifically within the `Flux.generate()` method, and it's not a standalone sink type like `FluxSink`. The `SynchronousSink` is used for generating elements in a synchronous manner within the generator function.