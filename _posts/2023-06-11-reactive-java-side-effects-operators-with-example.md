---
layout: post
title: 'Reactive Java: Side-effects Operators with Example'
date: '2023-06-11 14:32:50 +0530'
---

In Reactive Java, there are several side-effect operators available that allow you to perform actions or introduce side-effects at different points in the reactive stream. Here are some commonly used side-effect operators:

### `doOnNext`

The `doOnNext` operator allows you to perform an action for each item emitted by the stream, without modifying the item or the stream itself.

```java
Flux.range(1, 5)
    .doOnNext(item -> System.out.println("Received item: " + item))
    .subscribe();
```

### `doOnError`

The `doOnError` operator allows you to perform an action when an error occurs in the stream.

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

### `doOnComplete`

The `doOnComplete` operator allows you to perform an action when the stream completes successfully.

```java
Flux.range(1, 5)
    .doOnComplete(() -> System.out.println("Stream completed"))
    .subscribe();
```

### `doOnSubscribe` and `doOnCancel`

The `doOnSubscribe` operator allows you to perform an action when the stream is subscribed to, while the `doOnCancel` operator allows you to perform an action when the subscription is canceled.

```java
Flux.range(1, 5)
    .doOnSubscribe(subscription -> System.out.println("Subscribed"))
    .doOnCancel(() -> System.out.println("Canceled"))
    .subscribe()
    .dispose(); // Cancels the subscription
```

### `doOnEach`

The `doOnEach` operator allows you to perform an action for each item emitted by a reactive stream, including signals such as `onNext`, `onError`, and `onComplete`. It is a versatile operator that allows you to add side-effects or perform additional operations at different points in the stream.

```java
Flux.range(1, 5)
    .doOnEach(signal -> {
        if (signal.getType() == SignalType.ON_NEXT) {
            int item = signal.get();
            System.out.println("Received item: " + item);
        } else if (signal.getType() == SignalType.ON_ERROR) {
            Throwable error = signal.getThrowable();
            System.out.println("Error occurred: " + error.getMessage());
        } else if (signal.getType() == SignalType.ON_COMPLETE) {
            System.out.println("Stream completed");
        }
    })
    .subscribe();
```

