---
title: "Decoding the Mysteries of Java’s InaccessibleObjectException: Your In-Depth Guide"
date: 2023-11-28 09:00:00 -0000
categories: [Java, java.base]
tags: [java, java-unchecked, java.lang.reflect, java-se]
mermaid: true
toc: true
---


As the dynamics of Java programming has evolved, the importance of understanding different exceptions, including the `InaccessibleObjectException`, cannot be overstated. This blog post will take you through a detailed discussion about `InaccessibleObjectException` in Java, showcasing the vital elements regarding its purpose, why it occurs, and how you can handle it in your Java project.

## What is InaccessibleObjectException?

`InaccessibleObjectException` was introduced in Java 9 as part of the Java Platform Module System (JPMS). It generally occurs when an application attempts to access a JVM (Java Virtual Machine) inaccessible object, typically during operations involving Reflection API.

The exception belongs to the `java.lang.reflect` package and extends the `RuntimeException`.

```java
public class InaccessibleObjectException
extends RuntimeException
```

To get the essence of how it comes into play, let's understand an important aspect of JPMS, encapsulation.

## Encapsulation in Java 9

Starting from Java 9, JPMS added strong encapsulation which could hide classes from being available on the classpath. Conceptually, JPMS aimed to contain the internal APIs, such as `sun.misc.Unsafe`, and encouraged developers to use only standard APIs. Often, this resulted in an `InaccessibleObjectException`.

The following is an example of this situation:

```java
import sun.misc.BASE64Encoder;

public class Example {
    public static void main(String[] args) {
        BASE64Encoder encoder = new BASE64Encoder();
        System.out.println(encoder.encode("Hello, World!".getBytes()));
    }
}
```

Running this code in Java 9 or a higher version will throw the `InaccessibleObjectException`, because the `sun.misc.BASE64Encoder` is not accessible.

## Instances of InaccessibleObjectException

### Reflection API

The Reflection API allows a program at runtime to view and alter the fields, methods, and classes. However, this capability can also lead to breaking the encapsulation. Java 9 introduced the concept of "Accessible" and "Inaccessible," making some objects inaccessible during reflection API operations.

Consider the code:

```java
import java.lang.reflect.Field;

public class Example {
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException {
        Field value = String.class.getDeclaredField("value");
        value.setAccessible(true); // this line will throw an error
        char[] chars = (char[]) value.get("Hello, world!");
        System.out.println(chars);
    }
}
```
The code uses reflection to access the private `value` field of the `String` class. Notably, trying to do this in Java 9 or higher versions will throw the `InaccessibleObjectException`.

## How to Handle InaccessibleObjectException?

The simplest approach of handling the `InaccessibleObjectException` is essentially to refrain from using the non-public APIs or accessibility features, such as `setAccessible()`. At times, resorting to the standard APIs is not an option, or you might really need the reflection functionality - in such cases, there are some workarounds to prevent this exception.

Let’s discuss one possible method, adding exemptions for specific modules:

```bash
--add-exports module/package=target-module(,target-module)*
```

In the case of the `String` class example, running JVM with `--add-exports java.base/java.lang=ALL-UNNAMED` will permit the module `java.base` to reflectively access the `java.lang` package.

```bash
java --add-exports java.base/java.lang=ALL-UNNAMED Example
```

## Conclusion

The `InaccessibleObjectException` is JDK's way of enforcing modularity and encapsulation in Java. While the exception may seem difficult at the start, understanding its workings can significantly benefit your programming skills, especially when writing complex enterprise-grade applications. 

Orderly encapsulation is the future of Java and learning to navigate these components allows us to build scalable, modular, and performant Java applications.

### References

1. `[InaccessibleObjectException Oracle Docs](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/InaccessibleObjectException.html)`
2. `[Java Platform Module System (JPMS)](https://www.oracle.com/corporate/features/understanding-java-9-modules.html)`
3. `[Java Reflection API](https://www.oracle.com/technical-resources/articles/java/javareflection.html)`