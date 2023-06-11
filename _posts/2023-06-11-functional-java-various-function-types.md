---
layout: post
title: 'Functional Java: Various Function Types'
date: '2023-06-11 12:43:20 +0530'
categories: [JAVA,FUNCTIONAL]
tags: [functional, java]
---

## Various Types of Functions

Functional Java provides various types of functions that support functional programming. These function types are defined as functional interfaces in the Java standard library. Here are some common types of functions in functional Java:

### `Function<T, R>`

This functional interface represents a function that takes **an argument of type** `T` and **returns a result of type** `R`. It is used to transform an input value into an output value. The **`apply`** method is defined in this interface to perform the computation.

```java
import java.util.function.Function;

public class FunctionExample {
    public static void main(String[] args) {
        Function<Integer, String> intToString = num -> "Number: " + num;

        System.out.println(intToString.apply(42)); // Number: 42
    }
}
```

### `Predicate<T>`

The `Predicate` functional interface represents a function that takes an argument of type `T` and returns a **boolean** value. It is commonly used for filtering elements or checking a condition. The **`test`** method is defined in this interface to evaluate the predicate.

```java
import java.util.function.Predicate;

public class PredicateExample {
    public static void main(String[] args) {
        Predicate<Integer> isEven = num -> num % 2 == 0;

        System.out.println(isEven.test(4)); // true
        System.out.println(isEven.test(7)); // false
    }
}
```

### `Consumer<T>`

The `Consumer` functional interface represents a function that takes an argument of type `T` and performs some action but **does not return any result**. It is often used for side-effecting operations like printing or modifying objects. The `accept` method is defined in this interface to consume the value.

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class ConsumerExample {
    public static void main(String[] args) {
        List<String> fruits = Arrays.asList("Apple", "Banana", "Orange");

        Consumer<String> printFruit = fruit -> System.out.println("Fruit: " + fruit);

        fruits.forEach(printFruit);
    }
}

```

### `Supplier<T>`

The `Supplier` functional interface represents a function that **does not take any arguments** but **produces a result** of type `T`. It is used to provide values on demand. The `get` method is defined in this interface to retrieve the value.

```java
import java.util.function.Supplier;

public class SupplierExample {
    public static void main(String[] args) {
        Supplier<Double> randomValue = () -> Math.random();

        System.out.println(randomValue.get()); // Random value between 0 and 1
    }
}

```

### `BiFunction<T, U, R>`

This functional interface represents a function that **takes two arguments** of types `T` and `U` and returns a result of type `R`. It is used for operations that involve two inputs and produce an output. The `apply` method is defined in this interface to perform the computation.

```java
import java.util.function.BiFunction;

public class BiFunctionExample {
    public static void main(String[] args) {
        BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;

        System.out.println(add.apply(4, 5)); // 9
    }
}
```

### `BiConsumer<T>`

The `BiConsumer` functional interface represents a function that **takes two arguments** of different types and performs an action on them. It **does not produce a result**. The `accept` method is defined in this interface to consume the value.

```java
import java.util.function.BiConsumer;

public class BiConsumerExample {
    public static void main(String[] args) {
        BiConsumer<String, Integer> printDetails = (name, age) -> {
            System.out.println("Name: " + name);
            System.out.println("Age: " + age);
        };

        printDetails.accept("John", 25);
    }
}

```

### `UnaryOperator<T>`

The `UnaryOperator` functional interface represents a function that **takes an argument of type** `T` and **returns a** **result of the same type** `T`. It is used for functions that perform transformations on a single input. The `apply` method is defined in this interface to perform the computation.

```java
import java.util.function.UnaryOperator;

public class UnaryOperatorExample {
    public static void main(String[] args) {
        UnaryOperator<Integer> square = num -> num * num;

        System.out.println(square.apply(5)); // 25
    }
}
```

### `BinaryOperator<T>`

The `BinaryOperator` functional interface represents a function that **takes two arguments** of type `T` and returns a **result of the same type** `T`. It is used for functions that operate on two inputs and produce a result. The `apply` method is defined in this interface to perform the computation.

```java
import java.util.function.BinaryOperator;

public class BinaryOperatorExample {
    public static void main(String[] args) {
        BinaryOperator<Integer> multiply = (a, b) -> a * b;

        System.out.println(multiply.apply(4, 5)); // 20
    }
}
```