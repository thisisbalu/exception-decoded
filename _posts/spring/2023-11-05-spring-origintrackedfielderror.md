---
title: "Unravelling the Mysteries of OriginTrackedFieldError in Spring Framework"
date: 2023-11-29 09:00:00 -0000
categories: [Spring, spring-boot]
tags: [spring, spring-unchecked, org.springframework.boot.context.properties.bind.validation]
mermaid: true
toc: true
---


Welcome to today's deep dive into one of the fascinating corners of the Spring framework - the `OriginTrackedFieldError`. This beast has perplexed numerous developers, beginners and seasoned pros alike. Don't worry though, by the end of this blog post, you will have a clear understanding of `OriginTrackedFieldError` and be able to handle it like a pro. 

## Introduction to Spring's FieldError

To truly understand `OriginTrackedFieldError`, we first need to comprehend its parent class - `FieldError`. In Spring Framework, `FieldError` is an object that encapsulates the details of validation error of a specific field. It includes the following information: 

- Object Name: The name of the object where the error occurred.
- Field: The name of the field where the error occurred.
- Rejected Value: The value that triggered the error.
- Default Message: A default message regarding the error.

A simple example of `FieldError` creation in Spring might look like this:

```java
FieldError error = new FieldError("user", "email", "invalid email address");
```

## Diving into OriginTrackedFieldError in Spring 

Now with a grasp of `FieldError`, we are ready to introduce `OriginTrackedFieldError`.

`OriginTrackedFieldError` is a subclass of `FieldError` class and fits into Spring's error handling and reporting mechanism. It carries all the information of `FieldError`, but with an additional capability - as the name implies, it has a feature to track the origin of the error. To put it simply, it comes in handy when you want to trace the root cause of the validation error. 

Here is how you can create an `OriginTrackedFieldError`:

```java
OriginTrackedFieldError error = new OriginTrackedFieldError("user", "email", "invalid email address", "email validation failed");
```
In this code, `"email validation failed"` is the origin of the error.

## When to Use OriginTrackedFieldError?

Consider you are working on a complex application with various validation rules placed across different classes and methods. If a validation error occurs, it could be a daunting task to find out where exactly the validation failed, especially when you have multiple layers and the validation error could be from any of these layers.

In such cases, the `OriginTrackedFieldError` helps you by providing the exact location of the validation failure, making the debugging a lot easier. 

## Conclusion

Understanding `Spring Framework` is a long and continuous journey. However, understanding its error handling, specifically `OriginTrackedFieldError`, will save you hours and hours of debugging time. It brings out the beauty of Spring's error handling - precise and descriptive. So, the next time you face a validation error in your application, you know how to trace it back with `OriginTrackedFieldError`. Happy coding!

## References 

- [Spring Framework Documentation - Class FieldError](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/FieldError.html)
- [Spring Framework Detailed Project Structure](https://spring.io/projects/spring-framework)
- [Error Handling in Spring](https://www.baeldung.com/exception-handling-for-rest-with-spring)

If you face any challenges while implementing the `OriginTrackedFieldError`, feel free to seek help on [Stackoverflow](https://stackoverflow.com/questions/tagged/spring) or [Spring's Community](https://spring.io/community).