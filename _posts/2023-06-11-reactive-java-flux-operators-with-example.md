---
layout: post
title: 'Reactive Java: Flux - Operators with Example'
date: '2023-06-11 14:00:17 +0530'
categories: [JAVA,REACTIVE]
tags: [reactive, java, take, filter, map, flatMap, concatWith, reduce, zip, merge]
---

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

### `reduce` Operator

The `reduce` operator is used to combine all the elements emitted by a `Publisher` into a single value.

```java
import reactor.core.publisher.Flux;

public class ReduceOperatorExample {
    public static void main(String[] args) {
        Flux.range(1, 5)
            .reduce(0, (acc, value) -> acc + value) // Add all the numbers
            .subscribe(System.out::println);
    }
}
```

In this example, `Flux.range(1, 5)` generates a `Flux` that emits the numbers 1 to 5. Then, using the `reduce` operator, we accumulate all the emitted values by adding them together. The initial value is set to 0. Finally, we subscribe to the resulting `Mono` (since `reduce` returns a `Mono`) and print the reduced value using `System.out::println`.

The output of the above code will be:
```
15
```

As you can see, all the numbers are added together using the `reduce` operator.

### `zip` Operator

The `zip` operator is used to combine the elements emitted by multiple `Publishers` into pairs or tuples.

```java
import reactor.core.publisher.Flux;

public class ZipOperatorExample {
    public static void main(String[] args) {
        Flux<Integer> numbers = Flux.range(1, 5);
        Flux<Character> letters = Flux.just('A', 'B', 'C', 'D', 'E');

        Flux.zip(numbers, letters)
            .subscribe(tuple -> System.out.println(tuple.getT1() + " - " + tuple.getT2()));
    }
}
```

In this example, we have two `Publishers`: `numbers`, which emits the numbers 1 to 5, and `letters`, which emits the characters 'A' to 'E'. Using the `zip` operator, we combine the corresponding elements from both `Publishers` into tuples. Finally, we subscribe to the resulting `Flux` of tuples and print each tuple using `System.out::println`.

The output of the above code will be:
```
1 - A
2 - B
3 - C
4 - D
5 - E
```

As you can see, the numbers and letters are combined into pairs using the `zip` operator.

### `merge` Operator

The `merge` operator is used to combine the elements emitted by multiple `Publishers` into a single stream, preserving the order of emission.

```java
import reactor.core.publisher.Flux;

public class MergeOperatorExample {
    public static void main(String[] args) {
        Flux<Integer> numbers1 = Flux.range(1, 3);
        Flux<Integer> numbers2 = Flux.range(4, 3);

        Flux.merge(numbers1, numbers2)
            .subscribe(System.out::println);
    }
}
```

In this example, we have two `Publishers`: `numbers1`, which emits the numbers 1 to 3, and `numbers2`, which emits the numbers 4 to 6. Using the `merge` operator, we combine the elements from both `Publishers` into a single stream while preserving the order of emission. Finally, we subscribe to the merged `Flux` and print each element using `System.out::println`.

The output of the above code will be:
```
1
2
3
4
5
6
```

As you can see, the elements from both `Publishers` are merged into a single stream using the `merge`.