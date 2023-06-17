---
layout: post
title: 'Reactive Java: Batching and Grouping Approaches in Reactive Java'
date: '2023-06-17 10:55:47 +0530'
categories: [Reactive Java]
tags: [Java, Reactive Programming, Batching, Grouping]
---
## Introduction

In Reactive Java programming, batching and grouping are powerful techniques that allow for efficient processing and manipulation of data streams. Batching involves combining multiple elements into batches, while grouping involves categorizing elements based on a specific criterion. In this post, we will explore different batching and grouping approaches in Reactive Java and provide examples to illustrate their usages.

## Batching Approaches

### Approach 1: `buffer`

The `buffer` operator in Reactive Java is commonly used for batching elements from a `Flux` or `Mono`. It collects a specified number of elements or a specified time window's worth of elements and emits them as a `List` or a container type.

Here's an example of using `buffer` to batch elements into lists of size 3:

```java
import reactor.core.publisher.Flux;

Flux.range(1, 10)
    .buffer(3)
    .subscribe(batch -> System.out.println("Batch: " + batch));
```

The output of the above code will be:

```
Batch: [1, 2, 3]
Batch: [4, 5, 6]
Batch: [7, 8, 9]
Batch: [10]
```

### Approach 2: `window`

The `window` operator in Reactive Java is another approach for batching elements. Instead of emitting batches as lists, it creates new `Flux` instances for each batch.

Here's an example of using `window` to batch elements into separate `Flux` instances:

```java
import reactor.core.publisher.Flux;

Flux.range(1, 10)
    .window(3)
    .flatMap(batchFlux -> batchFlux.collectList())
    .subscribe(batch -> System.out.println("Batch: " + batch));
```

The output of the above code will be:

```
Batch: [1, 2, 3]
Batch: [4, 5, 6]
Batch: [7, 8, 9]
Batch: [10]
```

### Approach 3: Custom Batching

In some cases, you may need to implement custom batching logic based on specific requirements. Reactive Java provides various operators and functions that can be combined to achieve custom batching.

Here's an example of custom batching using `bufferUntil` and `bufferWhile`:

```java
import reactor.core.publisher.Flux;

Flux.range(1, 10)
    .bufferUntil(i -> i % 3 == 0)
    .subscribe(batch -> System.out.println("Batch: " + batch));

System.out.println();

Flux.range(1, 10)
    .bufferWhile(i -> i % 3 != 0)
    .subscribe(batch -> System.out.println("Batch: " + batch));
```

The output of the above code will be:

```
Batch: [1, 2, 3]
Batch: [4, 5, 6]
Batch: [7, 8, 9]
Batch: [10]

Batch: [1, 2]
Batch: [4, 5]
Batch: [7, 8]
Batch: [10]
```

## Grouping Approaches

### Approach 1: `groupBy`

The `groupBy` operator in Reactive Java allows you to group elements based on a specific criterion or key selector function. It returns a `Flux<GroupedFlux

<K, T>>` where `K` represents the grouping key and `T` represents the grouped elements.

Here's an example of using `groupBy` to group elements based on their parity (even or odd):

```java
import reactor.core.publisher.Flux;

Flux.range(1, 10)
    .groupBy(i -> i % 2 == 0 ? "even" : "odd")
    .subscribe(groupedFlux -> groupedFlux.collectList().subscribe(group -> System.out.println("Group: " + groupedFlux.key() + " - " + group)));
```

The output of the above code will be:

```
Group: even - [2, 4, 6, 8, 10]
Group: odd - [1, 3, 5, 7, 9]
```

### Approach 2: `windowUntil`

The `windowUntil` operator in Reactive Java allows you to group elements into separate `Flux` instances based on a condition. It creates new `Flux` instances whenever the condition is met.

Here's an example of using `windowUntil` to group elements until an element is divisible by 3:

```java
import reactor.core.publisher.Flux;

Flux.range(1, 10)
    .windowUntil(i -> i % 3 == 0)
    .flatMap(groupedFlux -> groupedFlux.collectList())
    .subscribe(group -> System.out.println("Group: " + group));
```

The output of the above code will be:

```
Group: [1, 2, 3]
Group: [4, 5, 6]
Group: [7, 8, 9]
Group: [10]
```

## Usage and Benefits

Batching and grouping approaches in Reactive Java offer several benefits:

1. **Efficiency**: Batching and grouping techniques allow for efficient processing and manipulation of data streams, optimizing resource usage.

2. **Data Organization**: Grouping elements based on specific criteria enables better organization and categorization of data.

3. **Modularity**: Batching and grouping operators can be combined with other Reactive Java operators to build complex data processing pipelines.

4. **Customization**: Reactive Java provides flexibility to implement custom batching and grouping logic based on specific requirements.

## Conclusion

Batching and grouping are essential techniques in Reactive Java for efficient processing and manipulation of data streams. The `buffer` and `window` operators provide convenient ways to batch elements, while the `groupBy` and `windowUntil` operators facilitate grouping based on specific criteria.

By understanding the different batching and grouping approaches and their usages, you can leverage these techniques in your Reactive Java applications to optimize resource usage, organize data, and perform complex data processing operations.