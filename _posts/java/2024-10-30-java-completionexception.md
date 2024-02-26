---
title: "The Complete Guide to CompletionException in Java"
date: 2024-10-30 09:00:00 -0000
categories: [Java, java.base]
tags: [java, java-unchecked, java.util.concurrent, java-se]
mermaid: true
toc: true
---


Have you ever encountered a `CompletionException` while working with Java? Don't worry, you're not alone! In this comprehensive guide, we'll explore what a `CompletionException` is, why it occurs, and how to handle it. So, fasten your seatbelts and let's dive into the world of `CompletionException`!

## What is a CompletionException?

A `CompletionException` is an unchecked exception that extends the `java.util.concurrent.ExecutionException` class in Java. It is thrown when a `Future` computation has been completed with an exception and that exception cannot be propagated directly.

In simpler terms, when you're working with `CompletableFuture` or any other asynchronous computation in Java, any exception thrown by the asynchronous task will be wrapped in a `CompletionException`.

## Why Do CompletionExceptions Occur?

CompletionExceptions typically occur in scenarios where an exception is thrown within an asynchronous task, and that exception is not properly handled. Let's consider an example to illustrate this:

```java
CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
    // Some task that may throw an exception
    throw new RuntimeException("Oops! Something went wrong.");
});

future.join();
```
In this example, we have a `CompletableFuture` that performs a computation asynchronously. In the `supplyAsync` method, we have a task that intentionally throws a `RuntimeException` to simulate an error. When we call `future.join()`, a `CompletionException` is thrown, wrapping the original exception.

## Handling CompletionExceptions

To handle a `CompletionException`, we need to unwrap it and retrieve the original, root cause exception. This can be achieved by using the `exceptionally` method provided by the `CompletableFuture` class:

```java
CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
    // Some task that may throw an exception
    throw new RuntimeException("Oops! Something went wrong.");
});

future.exceptionally(ex -> {
    if (ex instanceof CompletionException) {
        ex = ex.getCause();
    }
    
    // Handle the exception
    System.out.println("Error: " + ex.getMessage());
    return null;
}).join();
```

In the above code snippet, we chain a `exceptionally` method to the `CompletableFuture` instance. Inside the `exceptionally` block, we check if the exception is a `CompletionException` and then retrieve the root cause using `ex.getCause()`. Finally, we handle the exception by printing an error message and returning a default value (null in this case).

**Note:** It's important to handle `CompletionExceptions` effectively to avoid suppressing the root cause exception. In the example above, we simply print the error message, but you can choose to log it or take appropriate action based on your use case.

## Chaining Multiple CompletionExceptions

In real-world scenarios, you may need to perform a sequence of asynchronous tasks that depend on each other. In such cases, it's essential to handle `CompletionExceptions` safely, as they can occur at any stage in the chain.

Let's take a look at an example where we chain multiple `CompletableFuture` instances, each potentially throwing an exception:

```java
CompletableFuture<Integer> future1 = CompletableFuture.supplyAsync(() -> {
    // Some task that may throw an exception
    throw new RuntimeException("Oops! Something went wrong in future1.");
});

CompletableFuture<Integer> future2 = CompletableFuture.supplyAsync(() -> {
    // Some task that may throw an exception
    throw new RuntimeException("Oops! Something went wrong in future2.");
});

CompletableFuture<Integer> future3 = future1.thenCombine(future2, (result1, result2) -> {
    // Perform some computation with the results
    return result1 + result2;
});

future3.exceptionally(ex -> {
    if (ex instanceof CompletionException) {
        ex = ex.getCause();
    }
    
    // Handle the exception
    System.out.println("Error: " + ex.getMessage());
    return null;
}).join();
```

In this example, we have three `CompletableFuture` instances: `future1`, `future2`, and `future3`. We intentionally throw exceptions in the first two futures to simulate errors. We then chain `future1` and `future2` using the `thenCombine` method to perform a computation when both futures complete. Finally, we handle any `CompletionExceptions` that may occur during the execution of `future3`.

## Conclusion

`CompletionException` is a powerful construct in Java that helps handle exceptions raised during asynchronous computations. By properly handling `CompletionExceptions`, you can gracefully handle asynchronous errors and ensure your code remains robust.

In this article, we explored what a `CompletionException` is, why it occurs, and how to handle it effectively using `CompletableFuture`. By following the provided examples, you should now be able to confidently handle `CompletionExceptions` in your own Java code.

So, the next time you encounter a `CompletionException`, don't panic! Instead, embrace it as an opportunity to enhance your code resilience.

Happy coding!

## Reference Links

- [JavaDocs - CompletionException](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/CompletionException.html)
- [JavaDocs - CompletableFuture](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/CompletableFuture.html)

**Note:** The examples provided in this article have minimal error handling for brevity. In a real-world scenario, it is recommended to handle exceptions more robustly and take appropriate actions based on your application's requirements.

*This article is inspired by the Stack Overflow question: [How to properly handle CompletionException in Java?](https://stackoverflow.com/questions/43533103/how-to-properly-handle-completionexception-in-java)*