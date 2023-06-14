---
layout: post
title: 'Reactive Java: Flux concatMap vs flatMapSequential'
date: '2023-06-15 00:53:20 +0530'
categories: [JAVA,REACTIVE]
tags: [reactive, mono, flux, reactor]
---

Here are examples of using `concatMap` and `flatMapSequential` operators in Reactor:

### **concatMap**

```java
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

public class FluxConcatMapExample {
    public static void main(String[] args) {
        Flux<Integer> flux = Flux.range(1, 5);

        flux.concatMap(element -> {
            // Perform some operation on each element
            int modifiedElement = element * 2;

            // Return a new Flux or Mono based on the modified element
            return Flux.just(modifiedElement)
                    .subscribeOn(Schedulers.parallel());
        })
        .subscribe(System.out::println);
    }
}
```

In this example, `concatMap` is used to perform an operation on each element of the Flux. The `subscribeOn(Schedulers.parallel())` is added to ensure that the operations are executed in parallel using the parallel scheduler.

The output will be:
```
2
4
6
8
10
```

The elements are emitted in the same order as the original Flux because `concatMap` maintains the order of elements.

### **flatMapSequential**

```java
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

public class FluxFlatMapSequentialExample {
    public static void main(String[] args) {
        Flux<Integer> flux = Flux.range(1, 5);

        flux.flatMapSequential(element -> {
            // Perform some operation on each element
            int modifiedElement = element * 2;

            // Return a new Flux or Mono based on the modified element
            return Flux.just(modifiedElement)
                    .subscribeOn(Schedulers.parallel());
        })
        .subscribe(System.out::println);
    }
}
```

In this example, `flatMapSequential` is used to perform an operation on each element of the Flux. The `subscribeOn(Schedulers.parallel())` is added to execute the operations in parallel using the parallel scheduler.

The output will be:
```
2
4
6
8
10
```

Similar to `concatMap`, `flatMapSequential` maintains the order of elements, and the elements are emitted in the same order as the original Flux.

Both `concatMap` and `flatMapSequential` allow you to perform operations on elements emitted by a Flux while maintaining control over the execution order and concurrency. You can customize the operations inside the operator's function to suit your specific use case.