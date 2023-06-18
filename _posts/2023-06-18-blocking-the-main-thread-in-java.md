---
layout: post
title: 'Blocking the Main Thread in Java'
date: '2023-06-18 16:13:50 +0530'
categories: [Java, Multithreading]
tags: [Java, Multithreading, Concurrency]
---
## Introduction

In Java, the main thread is the thread that executes the main method of a program. By default, the main thread runs to completion and terminates the program. However, there may be cases where you need to block the main thread and prevent it from exiting immediately. In this post, we will explore the concept of blocking the main thread in Java, along with examples to illustrate its usage.

## Blocking the Main Thread

Blocking the main thread refers to the process of making the main thread pause its execution and wait for a certain condition to be met before continuing. This can be useful in scenarios where you want to keep the program running until a specific task or event completes.

## Example: Waiting for User Input

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("Enter your name: ");
        String name = scanner.nextLine();

        System.out.println("Hello, " + name + "!");

        // Block the main thread until Enter key is pressed
        System.out.println("Press Enter to exit...");
        scanner.nextLine();
    }
}
```

In this example, the main thread waits for user input by blocking on the `scanner.nextLine()` method. It prompts the user to enter their name, greets them, and then waits for the user to press Enter before exiting the program. By blocking the main thread, we ensure that the program remains running until the user explicitly decides to exit.

## Example: Waiting for Thread Completion

```java
class MyThread extends Thread {
    public void run() {
        // Perform some long-running task
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        MyThread thread = new MyThread();
        thread.start();

        // Block the main thread until the worker thread completes
        thread.join();

        System.out.println("Worker thread completed.");
    }
}
```

In this example, the main thread creates and starts a worker thread (`MyThread`). It then blocks using the `join` method, waiting for the worker thread to complete its execution. Once the worker thread finishes, the main thread resumes its execution and prints the completion message.

## Conclusion

Blocking the main thread can be useful in scenarios where you need to control the program's flow and ensure certain conditions are met before the program exits. Whether it's waiting for user input or waiting for the completion of other threads, blocking the main thread allows you to create interactive and synchronized Java applications.