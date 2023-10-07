---
title: "Understanding and Resolving MvcFoundOnClasspathException in Spring "
date: 2023-10-07 01:41:22 -0000
categories: [Spring, spring-cloud]
tags: [spring, spring-unchecked, org.springframework.cloud.gateway.support]
mermaid: true
toc: true
---


Spring, renowned for its simplicity and power, is often the go-to choice for developers when building enterprise-grade applications. Spring MVC is a powerful component of the Spring Framework that gives developers great flexibility. However, like any technology, it's not without its complications. One such hiccup is the MvcFoundOnClasspathException. 

In today's post, we'll dig into the MvcFoundOnClasspathException in Spring, from understanding what it is, when and why it arises, to the solution for resolving it through a step-by-step guide. 

## MvcFoundOnClasspathException: An Introduction

In Spring, the `MvcFoundOnClasspathException` is typically thrown by the `Spring Boot Auto Configuration` when Initializing the `DispatcherServletAutoConfiguration`. The `DispatcherServletAutoConfiguration` is the primary servlet of the Spring Web MVC framework. 

Essentially, this exception pops up when the Spring Boot Auto Configuration fails to detect the Spring MVC jar on the classpath during the project startup. 

Let’s look at a common scenario when the `MvcFoundOnClasspathException` might occur:

```java
@SpringBootApplication
public class MvcApplication { 
    public static void main(String[] args) { 
        SpringApplication.run(MvcApplication.class, args); 
    } 
}
```

When you run the above code without having Spring MVC jar in the classpath, the Spring Boot Auto Configuration throws the `MvcFoundOnClasspathException` and halts the application startup. 

## Diagnosing the MvcFoundOnClasspathException 

How can you confirm if it’s the `MvcFoundOnClasspathException` that's blocking your application? The best method is to check your error stack trace. It'll look something like this: 

```java
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'mvcHandlerMappingIntrospector'...
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'org.springframework.autoconfigure.web.servlet.WebMvcAutoConfiguration'...
Caused by: org.springframework.boot.autoconfigure.condition.ConditionEvaluationException: One or more Mvc types not present in classpath...
Caused by: org.springframework.boot.autoconfigure.condition.OnWebApplicationCondition$MvcFoundOnClasspathException: Unable to find DispatcherServlet or Mvc JavaConfig beans on the classpath...
```

## Resolving the MvcFoundOnClasspathException 

This error is simple to resolve once spotted. 

As the error informs that Spring MVC jar is missing, the solution is to add the following dependency in your Maven's `pom.xml` or Gradle's `build.gradle`:

```vbscript-xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.13.RELEASE</version> 
</dependency>   
```

Or if you're using Gradle:

```groovy
implementation 'org.springframework:spring-webmvc:5.2.13.RELEASE'
```

After adding this dependency,IDE like Eclipse or IntelliJ will download the jar and add it to the classpath itself. If not, execute a `mvn clean install` or `gradle clean build` for Maven and Gradle respectively, manually.

## Conclusion 

Spring is a potent tool in a developer's arsenal, but like any technology, it does have its share of complexities. The key lies in understanding these issues and finding a solution for them. The MvcFoundOnClasspathException is merely a speedbump in this journey, easily overcome with the right information.

Happy Coding! 

## References

1. [Spring MVC Guide - Official Documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html)
2. [Spring Boot Auto Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.autoconfiguration)
