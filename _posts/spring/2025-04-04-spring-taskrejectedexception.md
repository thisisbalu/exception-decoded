---
title: "Understanding TaskRejectedException in Spring Framework"
date: 2025-04-04 09:00:00 -0000
categories: [Spring, spring-framework]
tags: [spring, spring-unchecked, org.springframework.core.task]
mermaid: true
toc: true
---


The Spring Framework has revolutionized Java development by providing a comprehensive programming and configuration model. Among the various exceptions used in Spring, `TaskRejectedException` plays a significant role in managing task execution and handling thread pool behavior. This article will dive deep into `TaskRejectedException`, discussing what it is, when it occurs, and how to handle it effectively.

## What is TaskRejectedException?

`TaskRejectedException` is part of the Spring Framework and is thrown when a submitted task cannot be accepted for execution by a `ThreadPoolTaskExecutor`. This situation usually arises when the thread pool's limits have been reached, such as exceeding its maximum number of threads or its task queue capacity.

The `TaskRejectedException` extends `RejectionError`, which is part of the `java.util.concurrent` package. Consequently, it inherits common behaviors from Java's rejection policies and can be used to provide developers insights into handling task execution issues.

## Common Causes of TaskRejectedException 

1. **Maximum Pool Size Reached**: When the number of threads exceeds the `maxPoolSize` of your `ThreadPoolTaskExecutor`, any new tasks submitted will be rejected.
   
2. **Queue Capacity Exceeded**: The task queue may be full, and additional tasks cannot be accepted due to configured limits.

3. **Explicit Task Rejection Policies**: If a custom rejection policy is implemented, it can cause the exception when certain conditions are met.

### Example Configuration of ThreadPoolTaskExecutor

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

@Configuration
public class ExecutorConfig {
    
    @Bean
    public ThreadPoolTaskExecutor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(25);
        executor.setThreadNamePrefix("MyExecutor-");
        executor.initialize();
        return executor;
    }
}
```

### Triggering TaskRejectedException

Let’s explore a simple example where a `TaskRejectedException` might occur owing to the configuration provided above. In this scenario, we will submit more tasks than the maximum thread pool size and queue capacity.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;

@Service
public class TaskService {

    @Autowired
    private ThreadPoolTaskExecutor executor;

    @Async
    public void executeTask(int taskId) {
        System.out.println("Executing Task: " + taskId);
        try {
            Thread.sleep(2000); // Simulating long-running task
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public void submitTasks(int numberOfTasks) {
        for (int i = 0; i < numberOfTasks; i++) {
            executor.execute(() -> executeTask(i));
        }
    }
}
```

If you call `submitTasks(30)`, you can expect a `TaskRejectedException` once the limit set by the task executor is reached.

## Handling TaskRejectedException

### Default Rejection Policy in Spring

By default, the `ThreadPoolTaskExecutor` uses `AbortPolicy`, which throws a `TaskRejectedException` when tasks cannot be accepted. You can customize this behavior by setting a different policy.

### Custom Rejection Policy Example

Here’s how to implement and set a custom rejection policy that logs the rejection and can be an alternative to just throwing an exception.

```java
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;
import java.util.concurrent.RejectedExecutionHandler;

@Configuration
public class ExecutorConfig {

    @Bean
    public ThreadPoolTaskExecutor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(25);
        
        // Setting custom rejection policy
        executor.setRejectedExecutionHandler(new CustomRejectedExecutionHandler());
        executor.initialize();
        return executor;
    }

    class CustomRejectedExecutionHandler implements RejectedExecutionHandler {
        @Override
        public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
            System.out.println("Task " + r.toString() + " rejected");
        }
    }
}
```

This custom handler will log rejected tasks without throwing exceptions, allowing your application to keep running smoothly.

## Monitoring and Avoiding TaskRejectedException

To enhance performance and avoid `TaskRejectedException`, consider the following strategies:

1. **Tune Thread Pool Parameters**: Adjust `corePoolSize`, `maxPoolSize`, and `queueCapacity` based on your application's needs and expected load.
   
2. **Increase Queue Capacity**: If you anticipate a burst of tasks, increasing the queue size can mitigate immediate rejections.
   
3. **Review Task Execution Time**: Optimizing tasks to be shorter or more efficient will allow more tasks to be handled within the specified thread pool limits.

4. **Use Bounded Queue**: If you prefer stability over performance, a bounded queue can stall incoming tasks rather than reject them.

### Example of Bounded Queue Implementation

A bounded queue strategy would look similar to this:

```java
executor.setQueueCapacity(50);
executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
```

In this case, if the thread pool is full and a task is submitted, the calling thread will execute the task instead of throwing an exception.

## Conclusion 

`TaskRejectedException` serves as an essential aspect of managing concurrency in Spring applications. Understanding this exception, its causes, and potential resolutions is critical for developing robust and scalable Spring infrastructure. By leveraging the configuration capabilities of `ThreadPoolTaskExecutor` and implementing custom rejection policies, developers can monitor and adapt to application load better.

## References

- [Spring Official Documentation on Thread Pool Task Executor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/concurrent/ThreadPoolTaskExecutor.html)
- [Java Concurrency Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html)
- [Thread Pool Mechanism in Java](https://www.baeldung.com/java-thread-pool)
- [Handling Tasks and Concurrency in Spring](https://www.baeldung.com/spring-async)