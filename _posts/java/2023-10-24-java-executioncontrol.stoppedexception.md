---
title: "Master the ExecutionControl.StoppedException in JAVA: Uncover Its Nuances!"
date: 2023-11-02 09:00:00 -0000
categories: [Java, jdk.jshell]
tags: [java, java-unchecked, jdk.jshell.spi, jdk]
mermaid: true
toc: true
---


Are you a Java developer puzzled by the `ExecutionControl.StoppedException`? Have you ever come across this exception while running your Java code? If you answered yes to either of these questions, you're in the right place.

The `ExecutionControl.StoppedException` in Java often causes undue stress in developers who encounter it. But no worries, this comprehensive guide will explore the depths of the `ExecutionControl.StoppedException` in Java and enlighten you on its nuances so you can wield it to your advantage. 

## What Is the ExecutionControl.StoppedException in Java?

Before we dive into the deep end, let's cover some basics. `ExecutionControl.StoppedException` is a `RuntimeException` that, unlike checked exceptions, does not require to be declared in a method or constructor's `throws` clause. This exception class belongs to the `jdk.jshell` package, introduced in JDK 9 as a part of the JShell API.

The `ExecutionControl` class in JShell API controls the execution of snippets, and one of its subclasses is `StoppedException`. This exception is thrown when snippet execution is stopped irrespective of its normal or exceptional completion.

```java
public static class ExecutionControl.StoppedException
extends ExecutionControl.ExecutionControlException
```

## When Does It Occur?

The `ExecutionControl.StoppedException` is thrown when the execution of a snippet is forcibly stopped, either because JShell closes or due to a user-initiated event. For example, if you're running a loop in JShell and you manually stop it, it can throw this instance.

```java
for(int i=0; i<100; i++){
  System.out.println(i);
}
```
If the above code in JShell is forcibly stopped, it may throw the `ExecutionControl.StoppedException`.

## How to Handle ExecutionControl.StoppedException?

As this is a `RuntimeException`, we can either handle it using a try-catch or propagate it up the call stack. Here is how you can handle this exception:

```java
try {
  // snippet execution
} catch (ExecutionControl.StoppedException e) {
  // Exception handling
}
```

## Practical Usage of ExecutionControl.StoppedException

Although the practical usage of `ExecutionControl.StoppedException` is confined mainly to the JShell context, it's an interesting addition to Java's exception handling roster. Developers leveraging JShell in their applications can benefit from this exception by customizing their application's behavior when snippet execution is interrupted.

For example, you might want to log an informative message to users when they stop the execution of a long-running task:

```java
try {
    // code for executing long-running task
} catch (ExecutionControl.StoppedException e) {
    System.out.println("The task was stopped by user... IDLE mode activated");
    // switch to idle mode
}
```

## Important Note

The `jdk.jshell` package, and by extension the `ExecutionControl.StoppedException`, is not intended for use in general-purpose applications. Not only is it somewhat niche, but its stability is not guaranteed—it might be removed or changed in the future Java versions.

Also, remember that all the exceptions in the `ExecutionControl` class are not meant to be caught, and even if caught they should never be ignored.

For more insights on this exception and the jshell package as a whole, you can go through the [official Oracle documentation](https://docs.oracle.com/javase/9/docs/api/jdk/jshell/ExecutionControl.StoppedException.html).

## Wrapping Up

The `ExecutionControl.StoppedException` in Java might seem intimidating at first, but with the right understanding and usage, it can be another powerful tool in your Java toolkit. Embrace the nature of exceptions as your code's communication method for errors and disruptions, and always remember, exceptions are your allies – handle them with care!

Stay in tune, as our next posts will be covering more about Java exception handling, offering you tips, tricks, and best practices. Remember: when it comes to coding, knowledge is power!