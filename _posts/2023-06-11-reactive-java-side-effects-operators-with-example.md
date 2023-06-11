---
layout: post
title: 'Reactive Java: Side-effects Operators with Example'
date: '2023-06-11 14:32:50 +0530'
---

In Reactive Java, there are several side-effect operators available that allow you to perform actions or introduce side-effects at different points in the reactive stream. Here are some commonly used side-effect operators:

1. `doOnNext`: The `doOnNext` operator allows you to perform an action for each item emitted by the stream, without modifying the item or the stream itself.

```java
Flux.range(1, 5)
    .doOnNext(item -> System.out.println("Received item: " + item))
    .subscribe();
```

2. `doOnError`: The `doOnError` operator allows you to perform an action when an error occurs in the stream.

```java
Flux.range(1, 5)
    .map(item -> {
        if (item == 3) {
            throw new RuntimeException("Error occurred");
        }
        return item;
    })
    .doOnError(error -> System.out.println("Error occurred: " + error.getMessage()))
    .subscribe();
```

3. `doOnComplete`: The `doOnComplete` operator allows you to perform an action when the stream completes successfully.

```java
Flux.range(1, 5)
    .doOnComplete(() -> System.out.println("Stream completed"))
    .subscribe();
```

4. `doOnSubscribe` and `doOnCancel`: The `doOnSubscribe` operator allows you to perform an action when the stream is subscribed to, while the `doOnCancel` operator allows you to perform an action when the subscription is canceled.

```java
Flux.range(1, 5)
    .doOnSubscribe(subscription -> System.out.println("Subscribed"))
    .doOnCancel(() -> System.out.println("Canceled"))
    .subscribe()
    .dispose(); // Cancels the subscription
```

These side-effect operators provide a way to inject actions or perform additional operations at different points in the reactive stream. They are useful for logging, monitoring, resource management, or other side-effects that you may need in your application. However, it's important to use them judiciously, as excessive use of side-effects can reduce the clarity and maintainability of your code.