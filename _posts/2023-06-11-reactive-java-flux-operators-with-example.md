---
layout: post
title: 'Reactive Java: Flux - Operators with Example'
date: '2023-06-11 14:00:17 +0530'
---

## Reactive Java: Flux - Operators with Example

Here are some commonly used operators in Reactive Java's `Flux` class, along with examples of how they can be used:

### `map` Operator

The `map` operator allows you to transform each item emitted by a `Flux` by applying a function to it.

Example:
```java
Flux.range(1, 5)
    .map(item -> item * 2)
    .subscribe(System.out::println);
```
Output:
```
2
4
6
8
10
```

### `filter` Operator

The `filter` operator allows you to selectively emit items from a `Flux` based on a predicate.

Example:
```java
Flux.range(1, 5)
    .filter(item -> item % 2 == 0)
    .subscribe(System.out::println);
```
Output:
```
2
4
```

### `take` Operator

The `take` operator allows you to limit the number of items emitted by a `Flux`.

Example:
```java
Flux.range(1, 5)
    .take(3)
    .subscribe(System.out::println);
```
Output:
```
1
2
3
```

### `flatMap` Operator

The `flatMap` operator allows you to transform each item emitted by a `Flux` into a sequence of publishers, and then flatten the resulting sequences into a single `Flux`.

Example:
```java
Flux.range(1, 3)
    .flatMap(item -> Flux.just(item, item * 2))
    .subscribe(System.out::println);
```
Output:
```
1
2
2
4
3
6
```

### `concatWith` Operator

The `concatWith` operator allows you to concatenate two `Flux` instances together, emitting the items from the first `Flux` followed by the items from the second `Flux`.

Example:
```java
Flux<Integer> flux1 = Flux.range(1, 3);
Flux<Integer> flux2 = Flux.range(4, 3);

flux1.concatWith(flux2)
    .subscribe(System.out::println);
```
Output:
```
1
2
3
4
5
6
```
