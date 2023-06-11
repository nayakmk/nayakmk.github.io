---
layout: post
title: 'Functional Java: Stream Functions'
date: '2023-06-11 14:17:10 +0530'
---

## Functional Java: Stream Functions

In Java, the `java.util.stream` package provides a set of functional interfaces and methods to work with streams, which allow for efficient processing of collections and arrays in a declarative and functional style. Here are some commonly used stream functions in Java:

### `map`

The `map` function transforms each element of a stream using a given function and returns a new stream with the transformed elements.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> squaredNumbers = numbers.stream()
                                      .map(n -> n * n)
                                      .collect(Collectors.toList());
// squaredNumbers: [1, 4, 9, 16, 25]
```

### `filter`

The `filter` function selects elements from a stream based on a given predicate, and returns a new stream with the selected elements.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> evenNumbers = numbers.stream()
                                   .filter(n -> n % 2 == 0)
                                   .collect(Collectors.toList());
// evenNumbers: [2, 4]
```

### `reduce`

The `reduce` function performs a binary operation on the elements of a stream and returns an optional result.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream()
                 .reduce(0, (a, b) -> a + b);
// sum: 15
```

### `forEach`

The `forEach` function applies a given action to each element of a stream.

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.stream()
     .forEach(System.out::println);
// Output:
// Alice
// Bob
// Charlie
```

### `collect`

The `collect` function accumulates the elements of a stream into a collection or performs a mutable reduction.

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
String joinedNames = names.stream()
                          .collect(Collectors.joining(", "));
// joinedNames: "Alice, Bob, Charlie"
```

### `flatMap`

The `flatMap` function applies a transformation to each element of a stream and flattens the resulting sequences into a single stream.

```java
List<String> words = Arrays.asList("Hello", "World");
List<String> letters = words.stream()
                            .flatMap(word -> Arrays.stream(word.split("")))
                            .collect(Collectors.toList());
// letters: [H, e, l, l, o, W, o, r, l, d]
```

In this example, the `flatMap` function splits each word into an array of letters and then flattens the resulting sequences into a single stream.

### `distinct`

The `distinct` function filters out duplicate elements from a stream.

```java
List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 3, 4, 5, 5);
List<Integer> distinctNumbers = numbers.stream()
                                       .distinct()
                                       .collect(Collectors.toList());
// distinctNumbers: [1, 2, 3, 4, 5]
```

In this example, the `distinct` function removes duplicate numbers from the stream, resulting in a stream with unique elements.

### `sorted`

The `sorted` function sorts the elements of a stream based on a natural order or a provided comparator.

```java
List<Integer> numbers = Arrays.asList(5, 3, 1, 4, 2);
List<Integer> sortedNumbers = numbers.stream()
                                     .sorted()
                                     .collect(Collectors.toList());
// sortedNumbers: [1, 2, 3, 4, 5]
```

In this example, the `sorted` function arranges the numbers in ascending order.

You can combine these functions with other stream functions to perform complex data processing and transformations on your collections or arrays in a concise and functional manner.