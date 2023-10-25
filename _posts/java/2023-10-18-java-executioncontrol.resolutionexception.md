---
title: "Unveiling and Mastering the ExecutionControl.ResolutionException in Java"
date: 2023-10-18 21:21:13 -0000
categories: [Java, jdk.jshell]
tags: [java, java-unchecked, jdk.jshell.spi, jdk]
mermaid: true
toc: true
---


Java, a prominent programming language, represents an archetype for comprehensive exception handling mechanisms. Mastering these exception types is paramount in the journey of establishing oneself as an adept Java developer.

One of these exceptions, which stands significant yet often under-discussed in the developer's community, is the `ExecutionControl.ResolutionException`. This article aims to dissect and understand this exception, its triggers, and embrace good coding practices to manage it effectively.

Suitable for both beginners and intermediate Java programmers, this article provides step-by-step instructions padded with numerous code examples.

## Introduction: What is ExecutionControl.ResolutionException?

In brief, `ExecutionControl.ResolutionException` is a checked exception in Java that typically gets thrown when a `Snippet` subinterface cannot be found or resolved during a JShell evaluation.

Let's get a bit into JShell for those unfamiliar with it. JShell, featured in Java 9 onward, is an interactive tool for learning Java and protocol scripting. The tool allows for executing Java code snippet-by-snippet (i.e., REPL - Read Eval Print Loop functionality). Snippets include expressions, definitions, and declarations. 

Thus, the ExecutionControl.ResolutionException arises when JShell faces challenges finding or resolving snippet subinterfaces, resulting in unsuccessful evaluations.

## Causes and Occurrences

The `ExecutionControl.ResolutionException` typically occurs when:

- A referenced class or interface is not found during JShell's evaluation process.
- JShell fails to resolve referenced interfaces or classes due to classpath issues.

Let's illustrate this with an example. Consider executing the following code snippet in JShell:

```java
jshell> class Test { Test2 t;}
```

If the `Test2` class does not exist or is not within JShell's classpath, a `ResolutionException` will occur.

```java
jshell> Test t = new Test();
|  Error:
|  cannot find symbol
|    symbol:   class Test2
|  class Test { Test2 t;}
|              ^------^
```

## Handling ExecutionControl.ResolutionException

The best practice for handling exceptions in Java is to use try-catch blocks. Due to the checked nature of `ExecutionControl.ResolutionException`, it requires explicit handling.

Here's an illustration of a try-catch block to handle this exception:

```java
import jdk.jshell.*;

public class Test{
    public static void main(String[] args){
        JShell jshell = JShell.builder().build();
        String code = "class Test { Test2 t;}"; // This code will trigger the resolution exception
        try{
            jshell.eval(code);
        } catch(ResolutionException e){
            System.out.println("Could not resolve a reference in the code snippet");
            e.printStackTrace();
        }
    }
}
```

Mastering exceptions, including the `ExecutionControl.ResolutionException` in Java can help you avoid unwanted terminations, set up custom error messages, and facilitate debugging.

## Conclusion

Understanding `ExecutionControl.ResolutionException` entails a broader comprehension of JShell and its operation, given its nature as a checked exception that arises from unsuccessful JShell's evaluation.

Remember, a good foundation in exception handling is vital in Java. While this article focused on `ExecutionControl.ResolutionException`, there exist numerous Java exceptions that necessitate careful handling for efficient, bug-free code. Happy coding!

### References
1. [Java Docs - ExecutionControl.ResolutionException](https://docs.oracle.com/en/java/javase/11/docs/api/jdk.jshell/jdk/jshell/ExecutionControl.ResolutionException.html)
2. [Java Error Messages Documentation](https://docs.oracle.com/en/java/javase/11/jshell/error-messages.html)
3. [JShell Official Docs](https://docs.oracle.com/javase/9/jshell/)
4. [Oracle Blog - Introducing JShell](https://blogs.oracle.com/javamagazine/the-new-jshell-feature-in-java-9)
5. [Java Exception Handling Guide](https://www.javatpoint.com/exception-handling-in-java)
