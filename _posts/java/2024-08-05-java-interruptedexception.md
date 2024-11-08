---
title: "Understanding InterruptedException in Java"
date: 2024-08-05 09:00:00 -0000
categories: [Java, java.base]
tags: [java, java-checked, java.lang, java-se]
mermaid: true
toc: true
---


In the world of multithreaded programming, one of the common challenges developers face is dealing with thread interruptions. Java provides a mechanism to interrupt a thread by using the `InterruptedException` class. In this article, we will dive deep into `InterruptedException` in Java, understanding its purpose, usage, and best practices for handling it.

## Table of Contents

- [What is InterruptedException?](#what-is-interruptedexception)
- [Causes of InterruptedException](#causes-of-interruptedexception)
- [Handling InterruptedException](#handling-interruptedexception)
- [Best Practices](#best-practices)
- [Conclusion](#conclusion)
- [References](#references)

## What is InterruptedException?

The `InterruptedException` is a checked exception that gets thrown when a thread is interrupted while it is in a waiting or sleeping state. It is a way of communication between threads, allowing one thread to request the interruption of another. This exception allows threads to gracefully terminate their execution and unwind the stack, freeing up resources.

By convention, most blocking methods in Java, such as `Object.wait()`, `Thread.sleep()`, and `BlockingQueue.take()`, either directly or indirectly, throw `InterruptedException` when the thread is interrupted.

## Causes of InterruptedException

There are several scenarios when a thread can be interrupted in Java:

1. Calling the `Thread.interrupt()` method explicitly.
2. Another thread invokes `interrupt()` on the thread.
3. An interruption occurs during a blocking I/O operation.
4. The thread is waiting on a lock acquired using the `java.util.concurrent.locks.Lock` interface.

Let's explore each of these scenarios and understand them with code examples.

### 1. Calling `Thread.interrupt()`

A thread can interrupt itself by calling the `interrupt()` method. This usually happens when a thread wants to check if it has been interrupted and terminate its execution gracefully.

```java
public class InterruptExample {

    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                // Perform some task

                try {
                    Thread.sleep(1000); // Simulate some work
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt(); // Restore interrupted status
                }
            }
        });

        thread.start();

        // Interrupt the thread after 5 seconds
        try {
            Thread.sleep(5000);
            thread.interrupt();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

In the above example, we create a new thread and interrupt it after 5 seconds. The thread periodically checks for interruption using `Thread.currentThread().isInterrupted()` and gracefully terminates when interrupted.

### 2. Another Thread invoking `interrupt()`

A different thread can interrupt a thread by invoking the `interrupt()` method on it. This scenario is often used in multi-threaded applications where one thread wants to interrupt the execution of other threads.

```java
public class AnotherThreadExample {

    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                // Perform some task

                try {
                    Thread.sleep(1000); // Simulate some work
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        });

        Thread otherThread = new Thread(() -> {
            try {
                Thread.sleep(5000);
                thread.interrupt(); // Interrupt the thread after 5 seconds
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        thread.start();
        otherThread.start();
    }
}
```

In this example, we have two threads - `thread` and `otherThread`. The `otherThread` waits for 5 seconds and then interrupts the `thread`. The interrupted thread gracefully terminates its execution upon interruption.

### 3. Interruption during Blocking I/O

When a thread is performing a blocking I/O operation, such as reading from a `java.io.InputStream` or writing to a `java.io.OutputStream`, it can get interrupted by closing the underlying stream or socket.

```java
public class BlockingIOExample {

    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            try(InputStream inputStream = new FileInputStream("path/to/file.txt")) {
                int data;
                while ((data = inputStream.read()) != -1) {
                    // Process the data
                    
                    if (Thread.currentThread().isInterrupted()) {
                        throw new InterruptedException();
                    }
                }
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                e.printStackTrace();
            }
        });

        thread.start();

        // Interrupt the thread after 5 seconds
        try {
            Thread.sleep(5000);
            thread.interrupt();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

In this example, the thread reads data from a file using a `FileInputStream`. If the thread gets interrupted during the blocking `inputStream.read()` call, it throws an `InterruptedException`, which we catch, restore the interrupt status, and handle appropriately.

### 4. Interruption during Lock Acquisition

Threads can get interrupted while waiting on a lock acquired using the `java.util.concurrent.locks.Lock` interface. This can happen when another thread invokes the `Lock.lockInterruptibly()` method, interrupting the waiting thread.

```java
public class LockInterruptExample {

    private static final Lock lock = new ReentrantLock();

    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            try {
                lock.lockInterruptibly(); // Acquire lock
                // Perform some task
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                e.printStackTrace();
            } finally {
                if (Thread.currentThread().isInterrupted()) {
                    // Do cleanup or rollbacks
                }
                lock.unlock(); // Release the lock
            }
        });

        Thread otherThread = new Thread(() -> {
            try {
                Thread.sleep(5000);
                thread.interrupt(); // Interrupt the thread after 5 seconds
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        thread.start();
        otherThread.start();
    }
}
```

In this example, we have two threads - `thread` and `otherThread`. The `thread` tries to acquire a lock using `lock.lockInterruptibly()`. The `otherThread` waits for 5 seconds and then interrupts the `thread`. The interrupted thread releases the lock and performs any necessary cleanups.

## Handling InterruptedException

When a thread throws an `InterruptedException`, it indicates that it has been interrupted and should be terminated gracefully. Here are a few tips for handling `InterruptedException`:

- **Restore the Interrupted Status**: Upon catching an `InterruptedException`, it is crucial to restore the interrupted status of the thread by calling `Thread.currentThread().interrupt()`. Failing to do so may clear the interrupted status, leading to incorrect behavior in subsequent code.

- **Terminate Execution**: After restoring the interrupted status, it is typically recommended to terminate the thread's execution gracefully. You should break out of any loops and perform any cleanups or rollbacks necessary before exiting.

- **Clean Up Resources**: It is good practice to clean up any acquired resources before terminating the thread. The `finally` block can be used for releasing locks, closing streams, or deallocating resources.

Here's an example showcasing the recommended handling of `InterruptedException`:

```java
Thread thread = new Thread(() -> {
    try {
        while (!Thread.currentThread().isInterrupted()) {
            // Perform some task

            try {
                Thread.sleep(1000); // Simulate some work
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt(); // Restore interrupted status
                break;  // Terminate execution upon interruption
            }
        }

        // Clean up resources

    } finally {
        // Release locks, close streams, deallocate resources
    }
});

thread.start();
```

By adhering to these best practices, you can effectively handle `InterruptedException` and ensure your threads are terminated gracefully.

## Best Practices

To make the most out of `InterruptedException` in Java, here are some best practices to follow:

1. **Be Responsive to Interruptions**: Design your code to be responsive to interruptions by checking `Thread.currentThread().isInterrupted()` or using methods like `Thread.interrupted()`. Handle interruptions at appropriate points in your code.

2. **Avoid Swallowing InterruptedException**: It is crucial not to ignore `InterruptedException` without handling it properly. Ignoring this exception can lead to unexpected behavior and make it difficult to identify the cause of issues.

3. **Use Non-Blocking Alternatives**: When possible, prefer non-blocking alternatives over the blocking methods that communicate interruptions via `InterruptedException`. For example, instead of `Thread.sleep()`, use `java.util.concurrent.TimeUnit.sleep()`.

4. **Consider Thread Safety**: Ensure thread safety when handling interruptions. Synchronize access to shared variables and use appropriate concurrency mechanisms like locks to avoid race conditions.

## Conclusion

In this article, we explored `InterruptedException` in Java, its purpose, and the various scenarios in which it occurs. We learned how to handle and work with `InterruptedException` in a safe and efficient manner, following best practices. By understanding `InterruptedException` and its best practices, you can better handle thread interruptions and improve the reliability of your multithreaded Java applications.

## References

1. [Java Thread API](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Thread.html)
2. [Interrupting a Thread](https://www.baeldung.com/java-thread-interrupt)
3. [Concurrency in Java](https://www.geeksforgeeks.org/interrupting-a-thread-in-java/)
4. [Java Concurrent Package](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/concurrent/package-summary.html)