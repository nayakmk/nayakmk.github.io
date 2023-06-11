---
layout: post
title: 'Functional Java: Optional Type'
date: '2023-06-11 15:24:05 +0530'
categories: [JAVA,FUNCTIONAL]
tags: [functional, java]
---

Common methods provided by the `Optional` class in Java, along with examples:

### `of`

The `of` method creates an `Optional` object with a non-null value.

```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        Optional<String> name = Optional.of("John");

        System.out.println(name.get()); // John
    }
}
```

### `empty`

The `empty` method creates an empty `Optional` object with no value.

```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        Optional<String> emptyOptional = Optional.empty();

        System.out.println(emptyOptional.isPresent()); // false
    }
}
```

### `ofNullable`

The `ofNullable` method creates an `Optional` object that may hold a null value.

```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        String nullableValue = null;
        Optional<String> optionalValue = Optional.ofNullable(nullableValue);

        System.out.println(optionalValue.isPresent()); // false
    }
}
```

### `isPresent`

The `isPresent` method checks if the `Optional` object holds a value.

```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        Optional<String> name = Optional.of("John");

        System.out.println(name.isPresent()); // true
    }
}
```

### `get`

The `get` method retrieves the value held by the `Optional` object if it is present. Note that this method should be used with caution, as it throws a `NoSuchElementException` if the `Optional` is empty.

```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        Optional<String> name = Optional.of("John");

        System.out.println(name.get()); // John
    }
}
```

### `orElse`

The `orElse` method retrieves the value held by the `Optional` object if it is present, or returns a default value if the `Optional` is empty.

```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        Optional<String> name = Optional.empty();

        System.out.println(name.orElse("Unknown")); // Unknown
    }
}
```

### `orElseGet`

The `orElseGet` method retrieves the value held by the `Optional` object if it is present, or generates a default value using a `Supplier` if the `Optional` is empty.

```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        Optional<String> name = Optional.empty();

        String defaultValue = name.orElseGet(() -> "Unknown");
        System.out.println(defaultValue); // Unknown
    }
}
```

### `orElseThrow`

The `orElseThrow` method retrieves the value held by the `Optional` object if it is present, or throws a specified exception if the `Optional` is empty.

```java
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        Optional<String> name = Optional.empty();

        try {
            String value = name.orElseThrow(() -> new RuntimeException("Value is not present"));
        } catch (RuntimeException e) {
            System.out.println(e.getMessage()); // Value is not present
        }
    }
}
```
