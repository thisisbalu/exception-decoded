---
title: "Understanding RejectedExecutionException in Java"
date: 2024-09-17 09:00:00 -0000
categories: [Java, java.base]
tags: [java, java-unchecked, java.util.concurrent, java-se]
mermaid: true
toc: true
---


As a Java developer, you may have encountered the dreaded `RejectedExecutionException` at some point in your coding journey. This exception often leaves many developers scratching their heads, wondering how to handle it effectively. In this comprehensive guide, we will delve into the depths of this exception, understanding what it is and the best practices to handle it gracefully.

## Table of Contents

- [Introduction to RejectedExecutionException](#introduction-to-rejectedexecutionexception)
- [Causes of RejectedExecutionException](#causes-of-rejectedexecutionexception)
- [Common Scenarios triggering RejectedExecutionException](#common-scenarios-triggering-rejectedexecutionexception)
- [Handling RejectedExecutionException](#handling-rejectedexecutionexception)
- [Code Examples](#code-examples)
- [Conclusion](#conclusion)
- [References](#references)

## Introduction to RejectedExecutionException

The `RejectedExecutionException` is a runtime exception that belongs to the java.util.concurrent package. It occurs when an ExecutorService rejects the execution of a task. An ExecutorService manages thread execution for various tasks, but sometimes it may reject executing a particular task due to resource limitations or other factors.

## Causes of RejectedExecutionException

The RejectedExecutionException can occur due to several reasons, including:

1. **ThreadPool saturation**: When the ExecutorService reaches its maximum limit for thread creation, it cannot accommodate any more tasks. As a result, it rejects any additional tasks submitted to it.

2. **Shutdown of ExecutorService**: If you attempt to submit a task to an ExecutorService that has already been shut down, the ExecutorService will reject execution by throwing the RejectedExecutionException.

## Common Scenarios triggering RejectedExecutionException

Here are a few common scenarios where you might encounter RejectedExecutionException:

1. **Bursty Workloads**: In situations where there is a sudden spike in workload, the ExecutorService may not have enough capacity to handle all the incoming tasks. This can lead to the rejection of some tasks, resulting in a RejectedExecutionException.

2. **Misconfiguration of ThreadPool**: If the ExecutorService is not correctly configured with an appropriate thread pool size, it may quickly reach its maximum limit, causing the rejection of incoming tasks.

3. **Improper shutdown sequence**: If you shut down the ExecutorService before all submitted tasks are completed, any remaining tasks will be rejected and result in a RejectedExecutionException.

## Handling RejectedExecutionException

Handling the RejectedExecutionException gracefully is crucial to ensure the stability and reliability of your application. Here are some best practices to consider when dealing with this exception:

1. **Catch and log the exception**: Surrounding the code block that submits a task to the ExecutorService with a try-catch block will allow you to catch the RejectedExecutionException and log it for further investigation. By logging the exception and its stack trace, you can diagnose and resolve the underlying issue.

```java
try {
    executorService.execute(task);
} catch (RejectedExecutionException e) {
    logger.error("Task rejected: " + e.getMessage());
}
```

2. **Retry or fallback mechanism**: Implementing a retry mechanism can help handle temporary spikes in workload or resource limitations. For example, if you receive a RejectedExecutionException, you can retry with a delay or fallback to an alternative processing method until the ExecutorService becomes available again.

3. **Graceful shutdown**: Ensure you properly shut down the ExecutorService when it is no longer needed. This involves stopping the submission of new tasks and waiting for the completion of all currently executing tasks. Graceful shutdown avoids RejectedExecutionException by giving enough time for all tasks to finish execution before shutdown.

```java
executorService.shutdown();
try {
    if (!executorService.awaitTermination(60, TimeUnit.SECONDS)) {
        executorService.shutdownNow();
    }
} catch (InterruptedException e) {
    executorService.shutdownNow();
    Thread.currentThread().interrupt();
}
```

## Code Examples

Here, we provide a simple code example illustrating the usage of ExecutorService and handling RejectedExecutionException:

```java
ExecutorService executorService = Executors.newFixedThreadPool(5);

for (int i = 0; i < 10; i++) {
    try {
        executorService.execute(() -> System.out.println("Executing task"));
    } catch (RejectedExecutionException e) {
        System.err.println("Task rejected: " + e.getMessage());
    }
}

executorService.shutdown();
```

In the above example, we create a fixed thread pool with a maximum of 5 threads. We then submit 10 tasks to the ExecutorService. If any of the tasks get rejected, we catch the RejectedExecutionException and log an error message.

## Conclusion

In this guide, we explored the RejectedExecutionException in Java, understanding its causes and common scenarios triggering it. We also discussed best practices for handling this exception gracefully, including catching and logging the exception, implementing retry or fallback mechanisms, and ensuring a graceful shutdown of the ExecutorService.

By understanding and effectively handling RejectedExecutionException, you can ensure the resilience of your concurrent Java applications.

## References

1. [RejectedExecutionException - Java API Documentation](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/RejectedExecutionException.html)
2. [Thread Pools - Java Concurrency in Practice](https://jcip.net/)

---

Did you find this article helpful? Let us know in the comments below!

[Read more articles on our technical blog](https://yourtechnicalblog.com) to enhance your Java programming skills.