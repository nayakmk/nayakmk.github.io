---
layout: post
title: 'Reactive Java: Flux to Mono'
date: '2023-06-14 23:27:31 +0530'
categories: [GIT]
tags: [commands]
---

To convert a reactive Flux into a Mono, you can use operators such as `single()`, `next()`, or `last()`. These operators allow you to extract a single value from the Flux and convert it into a Mono. Here are some examples:

### Example 1: Using `single()`

```java
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

public class FluxToMonoExample {
    public static void main(String[] args) {
        Flux<Integer> flux = Flux.just(1, 2, 3, 4, 5);

        Mono<Integer> mono = flux.single();

        mono.subscribe(System.out::println);
    }
}
```

In this example, we start with a Flux that emits multiple values (1, 2, 3, 4, 5). We use the `single()` operator on the Flux to convert it into a Mono that emits a single value, which is the only value emitted by the original Flux. Finally, we subscribe to the Mono and print the emitted value.

The output will be:
```
1
```

### Example 2: Using `next()`

```java
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

public class FluxToMonoExample {
    public static void main(String[] args) {
        Flux<String> flux = Flux.just("Apple", "Banana", "Cherry");

        Mono<String> mono = flux.next();

        mono.subscribe(System.out::println);
    }
}
```

In this example, we have a Flux that emits three strings: "Apple", "Banana", and "Cherry". We use the `next()` operator on the Flux to convert it into a Mono that emits only the first value emitted by the Flux. Finally, we subscribe to the Mono and print the emitted value.

The output will be:
```
Apple
```

### Example 3: Using `last()`

```java
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

public class FluxToMonoExample {
    public static void main(String[] args) {
        Flux<Double> flux = Flux.just(1.2, 2.3, 3.4, 4.5);

        Mono<Double> mono = flux.last();

        mono.subscribe(System.out::println);
    }
}
```

In this example, the Flux emits four double values. We use the `last()` operator on the Flux to convert it into a Mono that emits only the last value emitted by the Flux. Finally, we subscribe to the Mono and print the emitted value.

The output will be:
```
4.5
```

These examples demonstrate how you can convert a Flux into a Mono using operators like `single()`, `next()`, or `last()`, depending on your specific use case and the behavior you want to achieve.