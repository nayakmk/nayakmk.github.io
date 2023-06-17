---
layout: post
title: 'Reactive Java: Grouping Data in Reactive Java with Group By'
date: '2023-06-17 07:54:25 +0530'
categories: [Reactive Java]
tags: [Java, Reactive Programming, Group By]
---
## Introduction

In Reactive Java programming, the `groupBy` operator is a powerful tool for grouping data based on a specific criteria. It allows you to group elements of a `Flux` or `Mono` into separate collections or streams based on a key selector function. In this post, we will explore the usage of the `groupBy` operator in Reactive Java with examples to illustrate its capabilities.

## Understanding the Group By Operator

The `groupBy` operator in Reactive Java groups the elements of a `Flux` or `Mono` based on a key selector function and returns a `Flux<GroupedFlux<K, V>>` or `GroupedMono<K, V>`, where `K` is the key type and `V` is the element type.

The resulting grouped flux or mono consists of individual groups, each represented by a `GroupedFlux` or `GroupedMono` instance, which provides access to the key and the grouped elements.

## Example 1: Grouping Numbers by Parity

```java
Flux<Integer> numbers = Flux.range(1, 10);

Flux<GroupedFlux<String, Integer>> groupedFlux = numbers.groupBy(number -> {
    if (number % 2 == 0) {
        return "even";
    } else {
        return "odd";
    }
});

groupedFlux.subscribe(group -> {
    String key = group.key();
    group.subscribe(number -> System.out.println(key + ": " + number));
});
```

In this example, we have a `Flux` called `numbers` that emits integers from 1 to 10. We use the `groupBy` operator to group the numbers based on their parity. If a number is even, it is assigned the key "even"; otherwise, it is assigned the key "odd".

The resulting `groupedFlux` is a `Flux` of `GroupedFlux` instances, where each `GroupedFlux` represents a collection of numbers with a specific key (either "even" or "odd").

We subscribe to the `groupedFlux` and for each group, we subscribe to the individual numbers within the group. We print the key along with each number to demonstrate the grouping.

The output of the above code will be:

```
even: 2
even: 4
even: 6
even: 8
even: 10
odd: 1
odd: 3
odd: 5
odd: 7
odd: 9
```

## Example 2: Grouping Employees by Department

```java
class Employee {
    private String name;
    private String department;

    // Constructor, getters, and setters
}

Flux<Employee> employees = Flux.just(
        new Employee("John", "Sales"),
        new Employee("Jane", "Marketing"),
        new Employee("Mike", "Sales"),
        new Employee("Emily", "Marketing")
);

Flux<GroupedFlux<String, Employee>> groupedFlux = employees.groupBy(Employee::getDepartment);

groupedFlux.subscribe(group -> {
    String department = group.key();
    group.subscribe(employee -> System.out.println(department + ": " + employee.getName()));
});
```

In this example, we have a `Flux` of `Employee` objects representing employees in an organization. Each employee has a name and a department. We use the `groupBy` operator to group the employees based on their department.

The resulting `groupedFlux` is a `Flux` of `GroupedFlux` instances, where each `GroupedFlux` represents a collection of employees with a specific department as the key.

We subscribe to the `groupedFlux` and for each group, we subscribe to the individual employees within the group. We print the department along with each employee's name to demonstrate the grouping.

The output of the above code will be:

```
Sales: John
Sales: Mike
Marketing: Jane
Marketing: Emily
```

## Usage and Benefits

The `groupBy` operator in Reactive Java offers several benefits:

1. **Grouping**: The `groupBy` operator allows you to efficiently group elements based on a specific criterion or key selector function.

2. **Flexibility**: You can group elements of any type, allowing for versatile data processing scenarios.

3. **Parallelism**: The grouped flux or mono can be processed in parallel, enabling concurrent processing of individual groups.

4. **Aggregate Operations**: You can apply various aggregate operations on the grouped flux or mono, such as reducing, filtering, mapping, and more.

## Conclusion

The `groupBy` operator in Reactive Java provides a powerful mechanism for grouping elements based on specific criteria. Whether you want to group numbers, objects, or any other type of data, the `groupBy` operator allows you to organize and process the elements efficiently in a reactive and concurrent manner.

By understanding the `groupBy` operator and its usage, you can leverage its capabilities to group and process data effectively in your Reactive Java applications.