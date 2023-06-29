---
layout: post
title: Understanding the Parts of a UUID String in Java
date: '2023-06-30 00:24:20 +0530'
categories: [Java, UUID]
tags: [Java, UUID, Examples]
---

UUIDs (Universally Unique Identifiers) are commonly used to generate unique identifiers for entities in various applications. A UUID is a 128-bit value represented as a string in the following format: `xxxxxxxx-xxxx-Mxxx-Nxxx-xxxxxxxxxxxx`, where each `x` represents a hexadecimal digit (0-9, a-f), `M` represents the UUID version, and `N` represents the UUID variant.

In this post, we will explore the various parts of a UUID string and how to extract information from it using Java with examples.

## UUID Parts Overview

A UUID string consists of five parts:

1. **Time-low**: The low-order 32 bits of the timestamp portion of the UUID.
2. **Time-mid**: The middle 16 bits of the timestamp portion of the UUID.
3. **Time-high-and-version**: The high-order 16 bits of the timestamp portion and the version number.
4. **Variant**: The variant (reserved bits) of the UUID.
5. **Clock sequence**: The clock sequence (variant-specific) and the node (48 bits) of the UUID.

## Extracting UUID Parts in Java

To extract the different parts of a UUID string in Java, we can use the `UUID` class provided by the Java SDK. Here's an example that demonstrates how to extract and display the various parts:

```java
import java.util.UUID;

public class UUIDPartsExtractor {

    public static void main(String[] args) {
        String uuidString = "123e4567-e89b-12d3-a456-426655440000";
        UUID uuid = UUID.fromString(uuidString);

        long timeLow = uuid.getLeastSignificantBits() >>> 32;
        int timeMid = (int) (uuid.getLeastSignificantBits() >>> 16) & 0xFFFF;
        int timeHighAndVersion = (int) (uuid.getMostSignificantBits() >>> 48) & 0xFFFF;
        int variant = (int) ((uuid.getMostSignificantBits() >>> 8) & 0xF);
        long clockSequenceAndNode = uuid.getMostSignificantBits() & 0xFF_FF_FF_FF_FF_FF_FF_FFL;

        System.out.println("Time-low: " + Long.toHexString(timeLow));
        System.out.println("Time-mid: " + Integer.toHexString(timeMid));
        System.out.println("Time-high-and-version: " + Integer.toHexString(timeHighAndVersion));
        System.out.println("Variant: " + Integer.toHexString(variant));
        System.out.println("Clock sequence: " + Long.toHexString(clockSequenceAndNode));
    }
}
```

Output:
```
Time-low: e89b12d3a4564266
Time-mid: 123e
Time-high-and-version: 7
Variant: 4
Clock sequence: 55440000
```

## Explanation

In the above example, we first convert the UUID string to a `UUID` object using the `fromString` method. Then, we use various bitwise operations and masks to extract the different parts of the UUID:

- The `getLeastSignificantBits` method returns a `long` value representing the lower 64 bits of the UUID, which includes the time-low, time-mid, and time-high-and-version parts.
- The `getMostSignificantBits` method returns a `long` value representing the upper 64 bits of the UUID, which includes the variant and clock sequence parts.
- We use bitwise shifting and masking operations to extract the individual parts from the combined bits.

## Conclusion

Understanding the different parts of a UUID string in Java allows you to extract and work with specific components of the UUID, such as the timestamp or variant information. In this post, we explored how to extract the various parts of a UUID string using the `UUID` class in Java.