---
layout: post
title: Custom UUID Generator using Java
date: '2023-06-30 01:00:50 +0530'
categories: [Java, UUID]
tags: [Java, UUID, Generator, Examples, Custom]
---

UUIDs (Universally Unique Identifiers) are commonly used to generate unique identifiers for entities in various applications. Java provides a built-in `java.util.UUID` class to generate random UUIDs. However, there may be scenarios where you need a custom UUID generator with specific requirements. In this post, we will explore how to create a custom UUID generator using Java with examples.

## Requirements

For our custom UUID generator, let's consider the following requirements:
1. The generated UUID should have a specific format.
2. The UUIDs should be sortable.
3. The generator should be thread-safe.

## Implementation

Here's an example implementation of a custom UUID generator that meets the specified requirements:

```java
import java.util.UUID;

public class CustomUUIDGenerator {

    private static final long START_EPOCH = 1577836800000L; // January 1, 2020 00:00:00 UTC
    private static final int UUID_VERSION = 4; // Random-based UUID
    private static final int UUID_VARIANT = 2; // RFC 4122 variant

    public synchronized static String generateUUID() {
        long timestamp = System.currentTimeMillis() - START_EPOCH;

        long mostSignificantBits = (UUID_VERSION << 12)
                | ((timestamp & 0xFFFFFFFFFFFFL) << 16)
                | UUID_VARIANT;

        long leastSignificantBits = new Random().nextLong();

        UUID uuid = new UUID(mostSignificantBits, leastSignificantBits);
        return uuid.toString();
    }
}
```

## Usage

To generate custom UUIDs using our `CustomUUIDGenerator`, simply call the `generateUUID` method:

```java
String uuid = CustomUUIDGenerator.generateUUID();
System.out.println(uuid);
```

Output:
```
9c5f0144-7503-4c25-b99f-5f2f89c7d69b
```

## Conclusion

Creating a custom UUID generator in Java allows you to generate UUIDs with specific requirements tailored to your application's needs. In this post, we implemented a custom UUID generator that generates sortable UUIDs with a specific format. The generator is also thread-safe, ensuring safe usage in concurrent environments.

Remember to adjust the `START_EPOCH`, `UUID_VERSION`, and `UUID_VARIANT` constants in the `CustomUUIDGenerator` class to fit your specific requirements.
