---
title: 'Mastering AssertionError in Java with Hands-on Examples'
date: 2023-09-20 21:02:32 -0000
categories: [Java, java.lang]
tags: [java, java-unchecked, java.base, java-se]
mermaid: true
toc: true
---


Assertions in Java is an interesting topic that might sound intimidating to a few. But no worries, this article aims to put you in complete control of `AssertionError` in Java, with easy to understand explanations and hands-on code examples.  

## What is AssertionError in Java?

`AssertionError` is an error thrown programmatically to indicate that an assertion has failed. An assertion is generally a condition that the programmer believes must be true at a certain point in a program. If an assertion evaluates to false during the execution of the program, an `AssertionError` gets thrown.

```java
public class AssertionExample {
    public static void main(String[] args) {
        int value = 15;
        assert value >= 20 : "Underweight";
        System.out.println("Pass");
    }
}
```
The above code will throw `AssertionError` because value is not >= 20.

## Using AssertionError in your code

`AssertionError` is an instance of `java.lang.Error`, which is a subclass of `java.lang.Throwable`. It's not intended to be caught or handled by the application. Instead, it should be used during development and testing to catch incorrect assumptions made in the code.

Here is an example of its usage:

```java
try {
    assert false;
} catch (AssertionError e) {
    e.printStackTrace();
}
```
While it is technically possible to catch and handle `AssertionError`, it's typically not good practice.

## When to use Assertions?

Assertions should be intended to catch your or other developer's mistakes. They should not be used to handle the flow of the program. For example, `IllegalArgumentException` or `NullPointerException` should be handled using exceptions, not assertions. 

It is not recommended to use assertions for verifying passageways of illegal arguments to public methods since the assertions can be disabled.

Consider the following example:

```java
public double calculateInterest(double principal, double rateOfInterest) {
    assert(principal > 0);
    assert(rateOfInterest > 0);
    // calculations
}
```
## Enabling and Disabling Assertions

By default, assertions are disabled in Java. We have to enable them using `-ea` or `-enableassertions` switch. 

Consider the following command:

```bash
java -ea com.example.AssertionExample
```
It would enable assertions for the `AssertionExample` class. 

We can also disable assertions using `-da` or `-disableassertions` switch.

Nested classes inherit assertion status from their parent class.

It's important to realize that assertions can be disabled, so never place a method call that changes the state of the program inside an assert statement. Because if assertions get disabled, the method would never get called leading to potential bugs.

## Best Practices with Assertions

1. Assertions should not replace unit tests. Instead, they should go hand in hand.
2. Assertions should not alter program state.
3. Assertions should not be used for argument checking in public methods.
4. Avoid catch blocks for `AssertionError`.
5. Always provide informative assertion failure messages.

## Conclusion

In this post, we have seen how to effectively use assertions in Java. As a best practice, remember that assertions are a powerful feature for verifying your private, undocumented assumptions about your program.

Happy Java hacking!

This Article Includes:
* [Oracle Assertion Documentation](https://docs.oracle.com/javase/7/docs/technotes/guides/language/assert.html)
* [Oracle Docs on Exceptions](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html)

```markdown
Disclaimer: As programming in Java demands careful attention to detail, `AssertionError` should be used appropriately and professionally to avoid unwanted program behavior. Always back your code with unit tests.
```