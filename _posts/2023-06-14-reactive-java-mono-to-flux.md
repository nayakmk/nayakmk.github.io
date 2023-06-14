---
layout: post
title: 'Reactive Java: Mono to Flux'
date: '2023-06-14 23:30:29 +0530'
categories: [JAVA,REACTIVE]
tags: [reactive, mono, flux, reactor]
---

To convert a reactive Mono into a Flux, you can use the `flux()` operator provided by the Reactor library. This operator creates a Flux that emits the value emitted by the Mono, followed by completion. Here's an example:

```java
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

public class MonoToFluxExample {
    public static void main(String[] args) {
        Mono<String> mono = Mono.just("Hello");

        Flux<String> flux = mono.flux();

        flux.subscribe(System.out::println);
    }
}
```

In this example, we start with a Mono that emits a single value, "Hello". We use the `flux()` operator on the Mono to convert it into a Flux. Finally, we subscribe to the Flux and print the emitted value.

When you run this code, the output will be:

```
Hello
```

This demonstrates how a Mono can be converted into a Flux with a single element.

You can also convert a Mono into a Flux with multiple elements by using operators like `flatMapMany()`. Here's an example:

```java
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

public class MonoToFluxExample {
    public static void main(String[] args) {
        Mono<Integer> mono = Mono.just(5);

        Flux<Integer> flux = mono.flatMapMany(n -> Flux.range(1, n));

        flux.subscribe(System.out::println);
    }
}
```

In this example, the initial Mono emits the value 5. We use the `flatMapMany()` operator on the Mono to convert it into a Flux that emits values from 1 to the emitted value (5 in this case). Finally, we subscribe to the Flux and print the emitted values.

The output will be:

```
1
2
3
4
5
```

This demonstrates how a Mono can be converted into a Flux with multiple elements using the `flatMapMany()` operator.