---
layout: post
title: 'Reactive Java: Understanding Flux Window and WindowTimeout Operators'
date: '2023-06-18 15:15:32 +0530'
---
## Introduction

In Reactive Java programming, Flux is a powerful component for working with streams of data. The `window` operator in Flux allows you to group elements emitted by the source Flux into windows or substreams. Additionally, the `windowTimeout` operator provides the capability to emit windows based on both size and duration conditions.

In this post, we will explore the `window` and `windowTimeout` operators in Flux and provide examples to demonstrate their usage.

## The `window` Operator

The `window` operator in Flux groups elements emitted by the source Flux into separate windows or substreams. Each window is represented by a nested Flux that emits the elements belonging to that window.

The signature of the `window` operator is as follows:

```java
Flux<Flux<T>> window(int maxSize)
```

The `maxSize` parameter specifies the maximum number of elements to include in each window. Once the window reaches this size, it is emitted as a nested Flux.

## The `windowTimeout` Operator

The `windowTimeout` operator in Flux combines both size and duration conditions to emit windows. It groups elements emitted by the source Flux into separate windows, and a window is emitted when either the size condition or the duration condition is met.

The signature of the `windowTimeout` operator is as follows:

```java
Flux<Flux<T>> windowTimeout(int maxSize, Duration timespan)
```

The `maxSize` parameter specifies the maximum number of elements to include in each window, and the `timespan` parameter specifies the maximum duration to wait before emitting the window, regardless of its size.

## Usage of `window` and `windowTimeout` Operators

Let's explore some examples to understand the usage of the `window` and `windowTimeout` operators in Flux.

### Example 1: Windowing Elements based on Size

```java
import reactor.core.publisher.Flux;

public class WindowSizeExample {

    public static void main(String[] args) {
        Flux.range(1, 10)
                .window(3)
                .flatMap(Flux::collectList)
                .subscribe(System.out::println);
    }
}
```

In this example, we have a Flux emitting elements from 1 to 10. We use the `window(3)` operator to group the elements into windows of size 3. The output will be a Flux of nested Fluxes, where each nested Flux represents a window containing 3 elements. We use `flatMap(Flux::collectList)` to convert each nested Flux into a List for easier readability.

```output
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]
[10]
```

### Example 2: Windowing Elements based on Size and Duration

```java
import reactor.core.publisher.Flux;
import java.time.Duration;

public class WindowTimeoutExample {

    public static void main(String[] args) {
        Flux.range(1,20)
                .windowTimeout(3, Duration.ofSeconds(5))
                .flatMap(Flux::collectList)
                .subscribe(System.out::println);

        try {
            Thread.sleep(15000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

In this example, we use the `range(1,20)` Flux to emit elements in range from 1 to 20. We use the `windowTimeout(3, Duration.ofSeconds(5))` operator to group the elements into windows of size 3 or when the duration of 5 seconds elapses, whichever condition is met first. The output will be a Flux of nested Fluxes, where each nested Flux represents a window containing the collected elements.

```output
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]
[10, 11, 12]
[13, 14, 15]
[16, 17, 18]
[19, 20]
```

## Conclusion

The `window` and `windowTimeout` operators in Flux provide flexible ways to group elements emitted by a Flux into separate windows or substreams based on different conditions. They allow for effective processing of data in windowed chunks, enabling you to perform specific operations on each window or substream individually.