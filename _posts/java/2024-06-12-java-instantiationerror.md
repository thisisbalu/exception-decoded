---
title: "InstantiationError in Java: Explained with Examples"
date: 2024-06-12 09:00:00 -0000
categories: [Java, java.base]
tags: [java, java-checked, java.lang, java-se]
mermaid: true
toc: true
---


## Introduction

Have you ever encountered the `InstantiationError` while working with Java? Don't worry, you're not alone! This error occurs when there is an issue with the instantiation of a class or interface. In this article, we will dive deep into what exactly an `InstantiationError` is, what causes it, and how to handle it effectively.

## What is an InstantiationError?

In Java, an `InstantiationError` is a runtime exception that occurs when trying to create an instance of a class or interface with an invalid or inaccessible constructor. It is thrown by the Java Virtual Machine (JVM) when it encounters a problem during the instantiation process.

The official documentation defines `InstantiationError` as:

> "Thrown when an application tries to use the Java `new` operator to create an instance of a class, but the specified class object cannot be instantiated because it is an abstract class or interface, or it has no accessible zero-argument constructor."

To better grasp the concept, let's explore several scenarios that can lead to an `InstantiationError`.

## Causes of InstantiationError

### Abstract Class Instantiation

One common cause of an `InstantiationError` is attempting to instantiate an abstract class. Abstract classes serve as blueprints for subclasses and cannot be directly instantiated. Let's look at an example:

```java
abstract class Shape {
    // Abstract methods
    public abstract void draw();
}

public class Main {
    public static void main(String[] args) {
        Shape shape = new Shape(); // InstantiationError: Shape is abstract
        shape.draw();
    }
}
```

In the above code, we define an abstract class `Shape` that contains an abstract method `draw()`. However, when we try to create an instance of `Shape`, the JVM throws an `InstantiationError` because abstract classes cannot be instantiated directly.

### Interface Instantiation

Similar to abstract classes, interfaces cannot be instantiated directly. They are meant to be implemented by classes. Let's consider the following example:

```java
interface Printable {
    void print();
}

public class Main {
    public static void main(String[] args) {
        Printable printable = new Printable(); // InstantiationError: Printable is abstract
        printable.print();
    }
}
```

In this case, we define an interface `Printable` that declares a method `print()`. As with abstract classes, an attempt to create an instance of the interface results in an `InstantiationError`.

### No Accessible Zero-Argument Constructor

An `InstantiationError` can also occur when trying to create an instance of a class that lacks a public zero-argument constructor. Take a look at the following example:

```java
class Employee {
    String name;
    int age;

    public Employee() {
        this.name = "John";
        this.age = 25;
    }

    public Employee(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class Main {
    public static void main(String[] args) {
        Employee employee = new Employee(); // InstantiationError: Employee.<init>()
        System.out.println(employee.name);
    }
}
```

The `Employee` class here has two constructors - one with parameters and one without parameters. As the zero-argument constructor is not declared as `public`, it cannot be accessed from outside the class. Consequently, an `InstantiationError` is thrown during instantiation.

## How to Handle InstantiationError?

When encountering an `InstantiationError`, it is crucial to identify and resolve the underlying issue. Here are some common strategies for handling this error:

### Review Class or Interface Modifiers

Ensure that the class or interface you are trying to instantiate is not marked as `abstract`. Abstract classes and interfaces cannot be instantiated directly.

### Check Constructor Accessibility

Verify that the class you are trying to instantiate has a public zero-argument constructor. If not, consider creating a public zero-argument constructor to resolve the issue.

### Implement Interfaces Correctly

Remember that interfaces cannot be instantiated directly. They are meant to be implemented by classes. Make sure you create an instance of a class that implements the desired interface.

## Conclusion

In this article, we explored the `InstantiationError` in Java and discussed its causes and potential solutions. We learned that `InstantiationError` occurs when trying to create an instance of an abstract class or interface or a class without an accessible zero-argument constructor. By reviewing class modifiers, constructor accessibility, and properly implementing interfaces, you can avoid or handle this error effectively.

To learn more about this topic, check out the following references:

- [Official Java Documentation: InstantiationError](https://docs.oracle.com/javase/9/docs/api/java/lang/InstantiationError.html)
- [Java Interface vs Abstract Class](https://www.baeldung.com/java-interface-vs-abstract-class)
- [Java Constructors: Zero-argument Constructor and Default Constructor](https://www.baeldung.com/java-constructors-zero-argument-default)

Happy coding!