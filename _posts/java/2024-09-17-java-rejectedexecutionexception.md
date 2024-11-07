---
title: "Catchy and SEO friendly title: Understanding RejectedExecutionException in Java: A Comprehensive Guide for Developers"
date: 2024-09-17 09:00:00 -0000
categories: [Java, java.base]
tags: [java, java-unchecked, java.util.concurrent, java-se]
mermaid: true
toc: true
---


[Reference](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/RejectedExecutionException.html)

## Introduction

In Java programming, it is common to encounter various exceptions while performing concurrent operations. One such exception is the RejectedExecutionException, which is a subclass of the RuntimeException. This exception typically occurs when a task is submitted to an Executor that has been shutdown or when the Executor's capacity has been exceeded. In this article, we will dive deep into the world of RejectedExecutionException, exploring its causes, characteristics, and solutions, to help you better handle this exception in your Java applications.

## Table of Contents

1. [What is the RejectedExecutionException?](#what-is-the-rejectedexecutionexception)
2. [Causes of the RejectedExecutionException](#causes-of-the-rejectedexecutionexception)
3. [Common Scenarios Triggering a RejectedExecutionException](#common-scenarios-triggering-a-rejectedexecutionexception)
4. [How to Handle a RejectedExecutionException](#how-to-handle-a-rejectedexecutionexception)
    1. [Using try-catch blocks](#using-try-catch-blocks)
    2. [Using the `RejectedExecutionHandler` interface](#using-the-rejectedexecutionhandler-interface)
    3. [Using a `ThreadPoolExecutor` with a custom RejectedExecutionHandler](#using-a-threadpoolexecutor-with-a-custom-rejectedexecutionhandler)
5. [Conclusion](#conclusion)
6. [Additional Resources](#additional-resources)

## What is the RejectedExecutionException?

The RejectedExecutionException is a runtime exception that indicates that the executor is not able to accept a task for execution. This exception is thrown when there are no more threads available in the executor to handle the submitted task or when the executor has been shutdown.

## Causes of the RejectedExecutionException

The RejectedExecutionException can occur due to the following reasons:

1. **Executor shutdown**: When an Executor has been shutdown using its `shutdown()` or `shutdownNow()` methods, any subsequent attempts to submit tasks to the Executor will result in a RejectedExecutionException.

    ```java
    ExecutorService executor = Executors.newFixedThreadPool(5);
    executor.shutdown();

    // Attempt to submit a task after shutdown
    executor.submit(() -> { // Throws RejectedExecutionException
        // Task code here
    });
    ```

2. **Executor capacity reached**: Executors have a capacity, which determines the maximum number of tasks they can handle concurrently. When the capacity of an executor has been reached, any further task submissions will result in a RejectedExecutionException.

    ```java
    ExecutorService executor = Executors.newFixedThreadPool(5);

    for (int i = 0; i < 10; i++) {
        // Attempt to submit more tasks than the executor capacity
        executor.submit(() -> { // Throws RejectedExecutionException
            // Task code here
        });
    }
    ```

## Common Scenarios Triggering a RejectedExecutionException

To further understand the RejectedExecutionException, let's explore some common real-life scenarios where this exception can occur:

1. **Web Server Overload**: When a web server receives a large number of concurrent requests, it typically uses an Executor to handle those requests concurrently. If the incoming request rate exceeds the capacity of the Executor, it can lead to a RejectedExecutionException.

2. **Scheduled Executor Shutdown**: In applications that use scheduled Executors for periodic task execution, if the scheduled Executor is shutdown and an attempt is made to schedule a new task, a RejectedExecutionException can be thrown.

## How to Handle a RejectedExecutionException

When encountering a RejectedExecutionException, there are several ways to handle it gracefully in your Java applications. Let's explore some of the commonly used approaches:

### Using try-catch blocks

The simplest way to handle a RejectedExecutionException is by wrapping the task submission inside a try-catch block. By doing so, you can catch the exception and handle it accordingly.

```java
ExecutorService executor = Executors.newFixedThreadPool(5);

try {
    executor.submit(() -> {
        // Task code here
    });
} catch (RejectedExecutionException e) {
    // Handle the exception
}
```

### Using the `RejectedExecutionHandler` interface

Java provides the `RejectedExecutionHandler` interface, which allows you to implement a custom handler for the RejectedExecutionException. You can pass an instance of this interface to an Executor to define your own behavior when a task is rejected.

```java
class CustomRejectedExecutionHandler implements RejectedExecutionHandler {
    @Override
    public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
        // Custom handling logic here
    }
}
```

### Using a `ThreadPoolExecutor` with a custom RejectedExecutionHandler

For more fine-grained control over task rejection in an Executor, you can use the `ThreadPoolExecutor` class along with a custom `RejectedExecutionHandler`. This allows you to define a specific behavior when a task is rejected, such as logging the rejected tasks or adding them to a queue for later execution.

```java
ThreadPoolExecutor executor = new ThreadPoolExecutor(
    5,
    10,
    1,
    TimeUnit.MINUTES,
    new ArrayBlockingQueue<>(100),
    new CustomRejectedExecutionHandler()
);
```

## Conclusion

The RejectedExecutionException is an important exception to handle in Java when dealing with concurrent programming. It can occur due to various reasons such as executor shutdown or executor capacity being exceeded. By understanding the causes and common scenarios triggering this exception, as well as the available techniques for handling it, you can ensure a more resilient and robust concurrent application.

In this article, we explored the RejectedExecutionException in detail, including its definition, causes, common scenarios, and techniques for handling it. Armed with this knowledge, you can now confidently tackle RejectedExecutionExceptions in your own Java applications and build more reliable concurrent systems.

## Additional Resources

- [Java Concurrent API Documentation](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/RejectedExecutionException.html)
- [Oracle Java Tutorials - Concurrency](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html)

Thank you for reading this comprehensive guide on RejectedExecutionException in Java. We hope it has been beneficial to you in your journey as a Java developer. Happy coding!