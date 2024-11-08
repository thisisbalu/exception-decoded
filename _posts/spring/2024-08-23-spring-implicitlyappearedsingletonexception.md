---
title: "ImplicitlyAppearedSingletonException in Spring: Understanding the Culprit Behind It"
date: 2024-08-23 09:00:00 -0000
categories: [Spring, spring-framework]
tags: [spring, spring-unchecked, org.springframework.beans.factory.support]
mermaid: true
toc: true
---


Have you ever encountered an "ImplicitlyAppearedSingletonException" while working with the Spring framework? If so, you're not alone. This exception is commonly faced by developers, especially those who are relatively new to Spring. In this article, we will delve deeper into this frequently encountered exception and shed light on the reasons behind its occurrence. 

## What is ImplicitlyAppearedSingletonException?
The `ImplicitlyAppearedSingletonException` is an exception that arises in the Spring framework when a singleton bean is requested by the application, but it hasn't been created yet. This usually happens when there is a circular dependency among Spring beans, causing a conflict in the initialization order.

### A Simple Example to Illustrate the Exception
Let's consider a scenario where we have two classes, `A` and `B`, which both depend on each other. Class `A` has a dependency on Class `B`, while Class `B` depends on Class `A`. When Spring tries to initialize one of these beans, it realizes that it needs the other bean that hasn't been created yet, resulting in the `ImplicitlyAppearedSingletonException` being thrown.

```java
public class A {
    private B b;
    
    public A(B b) {
        this.b = b;
    }
}

public class B {
    private A a;
    
    public B(A a) {
        this.a = a;
    }
}
```

## Understanding the Cause
Now that we have seen an example, let's understand the cause of this exception in more detail. 

When Spring initializes a bean, it first constructs the bean's dependencies to fulfill its dependency injection requirements. If any of these dependencies are also singletons and haven't been created yet, the `ImplicitlyAppearedSingletonException` is thrown since they are implicitly appearing while constructing the current bean.

## Resolving the ImplicitlyAppearedSingletonException
To resolve this exception, you must break the circular dependency cycle in your Spring beans. There are a few approaches you can consider:

### 1. Constructor Injection
Consider using constructor injection instead of setter injection, as it can help avoid circular dependencies. By injecting the dependencies through constructors, you ensure that all the required beans are available at the time of initialization.

Here's an updated version of our previous example that uses constructor injection:

```java
public class A {
    private B b;
    
    public A(B b) {
        this.b = b;
    }
}

public class B {
    private A a;
    
    public B(A a) {
        this.a = a;
    }
}
```

### 2. Setter Injection with `@Autowired` and `@Lazy`
If constructor injection is not feasible or you prefer using setter injection, you can annotate the setter method with `@Autowired` and `@Lazy` annotations. The `@Lazy` annotation ensures that the dependency is lazily initialized, which can help break the circular dependency cycle.

```java
public class A {
    private B b;
    
    @Autowired
    @Lazy
    public void setB(B b) {
        this.b = b;
    }
}

public class B {
    private A a;
    
    @Autowired
    @Lazy
    public void setA(A a) {
        this.a = a;
    }
}
```

### 3. Using `@DependsOn` Annotation
If neither constructor injection nor setter injection with `@Autowired` and `@Lazy` annotations is suitable, you can consider using the `@DependsOn` annotation. This annotation allows you to specify the bean dependencies explicitly.

```java
public class A {
    private B b;
    
    public A() {
    }
    
    @Autowired
    public void setB(B b) {
        this.b = b;
    }
}

public class B {
    private A a;
    
    public B() {
    }
    
    @Autowired
    @DependsOn("a")
    public void setA(A a) {
        this.a = a;
    }
}
```

## Conclusion
In this article, we explored the `ImplicitlyAppearedSingletonException` exception in the Spring framework. We discovered that this exception occurs due to a circular dependency among Spring beans, resulting in a conflict in the initialization order. To overcome this exception, we discussed three different approaches: constructor injection, setter injection with `@Autowired` and `@Lazy` annotations, as well as using the `@DependsOn` annotation.

By implementing these solutions, you can break the cycle and ensure that your Spring beans are initialized without encountering the `ImplicitlyAppearedSingletonException` again.

Remember, it's crucial to handle this exception proactively and design your beans in a way that avoids circular dependencies altogether.

Thank you for reading! If you have any questions or feedback, please feel free to reach out.

**References:**

- [Spring Framework Documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/)
- [ImplicitlyAppearedSingletonException - Stack Overflow](https://stackoverflow.com/questions/58959006/implicitlyappearedsingletonexception-when-adding-@cacheloader-on-@repository-cla)

*Estimated reading time: 15 minutes*