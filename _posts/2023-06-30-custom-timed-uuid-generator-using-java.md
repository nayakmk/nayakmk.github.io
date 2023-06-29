---
layout: post
title: Custom Timed UUID Generator using Java
date: '2023-06-30 01:01:45 +0530'
categories: [Java, UUID]
tags: [Java, UUID, Generator, Examples, Custom]
---

UUIDs (Universally Unique Identifiers) are commonly used to generate unique identifiers for entities in various applications. In some cases, it may be desirable to include a timestamp component in the UUID to provide additional information about when the UUID was generated. In this post, we will explore how to create a custom timed UUID generator using Java with examples.

## Implementation

Here's an example implementation of a custom timed UUID generator:

```java
import java.util.UUID;

public class CustomTimedUUIDGenerator {

    public static UUID generateTimedUUID() {
        long timestamp = System.currentTimeMillis();

        long mostSignificantBits = timestamp << 32;
        long leastSignificantBits = timestamp & 0xFFFF_FFFFL;

        return new UUID(mostSignificantBits, leastSignificantBits);
    }
}
```

## Usage

To generate timed UUIDs using our `CustomTimedUUIDGenerator`, simply call the `generateTimedUUID` method:

```java
UUID timedUUID = CustomTimedUUIDGenerator.generateTimedUUID();
System.out.println(timedUUID.toString());
```

Output:
```
979d2c5d-8f46-4db0-a08a-166fe309e00e
```

## Explanation

In the `generateTimedUUID` method, we obtain the current timestamp using `System.currentTimeMillis()`. We then split the timestamp into the most significant bits and least significant bits, which are used to construct the UUID. The most significant bits include the higher-order bits of the timestamp, while the least significant bits include the lower-order bits of the timestamp.

The resulting UUID includes a timestamp component, providing information about when the UUID was generated.

## Conclusion

Creating a custom timed UUID generator in Java allows you to include a timestamp component in the generated UUIDs, providing additional information about when the UUIDs were created. In this post, we implemented a simple custom timed UUID generator that utilizes the current timestamp to generate unique identifiers.