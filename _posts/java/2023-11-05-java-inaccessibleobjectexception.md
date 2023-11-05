---
title: "Unraveling the Jigsaw: Understanding InaccessibleObjectException in Java 9 and Beyond"
date: 2023-11-28 09:00:00 -0000
categories: [Java, java.base]
tags: [java, java-unchecked, java.lang.reflect, java-se]
mermaid: true
toc: true
---


Every seasoned Java developer has encountered and battled the various exceptions the programming language might throw their way, and `InaccessibleObjectException` is no exception. Introduced in Java 9, `InaccessibleObjectException` many somewhat perplex the Java community by their appearance. This article dives deep into understanding the reason behind their existence in the Java feature landscape and how to handle them proficiently.

## What is InaccessibleObjectException?

Introduced in the JDK9 as part of Java's Project Jigsaw (modular system), InaccessibleObjectException is a part of jdk.internal.reflect package. It's a well-intentioned barrier and a `RuntimeException` that occurs when a Java application tries to access a Java field, method, or class that's not officially exported or opened to that application's module.

```java
package java.lang.reflect;
public class InaccessibleObjectException extends RuntimeException {
        public InaccessibleObjectException(String msg) {
          super(msg);
        }
}
```

## When does the Exception occur?

You might encounter the `InaccessibleObjectException` when attempting to use reflection on a core JDK class. Let's consider the following example:

```java
import java.lang.String;

public class ExampleClass {

    public static void main(String[] args) throws Exception {

        Field valueField = String.class.getDeclaredField("value");
    
        valueField.setAccessible(true);

        char[] value = (char[]) valueField.get("Hello, world!");

        System.out.println(value);
    }
}
```

The code compiles without errors, however, during runtime, you'll receive the `InaccessibleObjectException` if running with JDK9 or latter versions. The reason being that the `value` field of the `String` class is not public API and is encapsulated from reflective access.

## Understanding it in the light of Java's Project Jigsaw

`InaccessibleObjectException` is directly tied to the culmination of Java's Project Jigsaw. The primary aim of Jigsaw was to make the Java SE platform, and the JDK more easily scalable down to small computing devices, improve security and performance, and make it easier for developers to construct and maintain libraries and large applications - this called for introducing the system of modules. 

## Working around InaccessibleObjectException

While the primary recommendation is towards revisiting your code and modifying it to not rely on internal APIs, there are workarounds provided for backward compatibility. 

You can grant your application proper access to the intended internal API by passing the JVM a command-line option. Let's modify the previous example:

```java
    -add-opens java.base/java.lang=ALL-UNNAMED
```

Re-running the code with the above JVM option allows reflective access to the internal `String` field and avoids the `InaccessibleObjectException`.

It's also worth noting this option is a temporary fix, and it's strongly advised to follow the migration path and reduce the usage of internal, non-documented elements. [JEP 260](https://openjdk.java.net/jeps/260) offers a roadmap for encapsulating most of the JDK's internal APIs.

## Conclusion

`InaccessibleObjectException` is a testament to the changing paradigm of Java with the advent of modules system aimed at improved security and maintainability. While it may steer developers into unfamiliar territory, honing your understanding of it provides both insight into the future of Java and strengthens your troubleshooting toolkit. Remember, as with most programming conundrums, InaccessibleObjectException is a code mentor in disguise teaching you to write better, future-proof Java code.

## References:
1. [Oracle JDK 9 Documentation](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/InaccessibleObjectException.html)
2. [Eclipse Documentation](https://www.eclipse.org/eclipse/news/4.7/jdk9.php)
3. [OpenJDK Project Jigsaw](http://openjdk.java.net/projects/jigsaw)
4. [JEP 260: Encapsulate Most Internal APIs](https://openjdk.java.net/jeps/260)