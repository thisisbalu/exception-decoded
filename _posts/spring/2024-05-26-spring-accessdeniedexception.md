---
title: "AccessDeniedException in Spring: Securing Your Application with Ease"
date: 2024-05-26 09:00:00 -0000
categories: [Spring, spring-security]
tags: [spring, spring-unchecked, org.springframework.security.access]
mermaid: true
toc: true
---


## Introduction
When it comes to application security, Spring Framework provides extensive features to protect your sensitive data and control access to different parts of your application. One of the key exceptions thrown by Spring Security is the `AccessDeniedException`, which occurs when a user tries to access a resource they don't have permission for. In this article, we'll dive deep into the `AccessDeniedException` in Spring, understanding what it is, how it's handled, and how you can leverage it effectively to secure your application.

## What is `AccessDeniedException`?

`AccessDeniedException` is an exception thrown by Spring Security when a user attempts to access a resource or perform an action that they are not authorized to. This exception indicates a denial of access and serves as a powerful mechanism to ensure the enforcement of your application's security policies.

## Handling `AccessDeniedException`

Handling `AccessDeniedException` requires configuring the appropriate mechanism to intercept the exception and take appropriate action. Spring Security provides several ways to handle this exception, including:

### 1. Customizing the Access Denied Page
To redirect users to a custom access denied page, you can configure the `AccessDeniedHandler` in your Spring Security configuration class:
```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .exceptionHandling()
                .accessDeniedHandler(new CustomAccessDeniedHandler());
    }
}
```
By implementing the `AccessDeniedHandler` interface in the `CustomAccessDeniedHandler` class, you can define your custom behavior in the `handle()` method:
```java
public class CustomAccessDeniedHandler implements AccessDeniedHandler {

    @Override
    public void handle(HttpServletRequest request,
                       HttpServletResponse response,
                       AccessDeniedException accessDeniedException) throws IOException, ServletException {
        // Custom logic to redirect to a specific access denied page
        response.sendRedirect("/access-denied");
    }
}
```

### 2. Throwing `AccessDeniedException` Manually
In some cases, you may need to manually throw the `AccessDeniedException` based on certain conditions. You can achieve this by injecting the `AccessDecisionManager` and calling its `decide()` method:
```java
@Autowired
private AccessDecisionManager accessDecisionManager;

public void checkAccess(User user, Resource resource) {
    // Check if the user has access to the resource
    if (!accessDecisionManager.decide(user, resource)) {
        throw new AccessDeniedException("Access denied");
    }
}
```

## Fine-tuning `AccessDeniedException`

Spring Security allows you to further customize the `AccessDeniedException` handling by configuring additional options.

### 1. AccessDeniedHandler for JSON Response
If your application primarily serves JSON responses, you might want to return a JSON error message when an access denied situation occurs. You can achieve this by implementing a custom `AccessDeniedHandler` to handle JSON responses:
```java
public class JsonAccessDeniedHandler implements AccessDeniedHandler {
    
    @Override
    public void handle(HttpServletRequest request,
                       HttpServletResponse response,
                       AccessDeniedException accessDeniedException) throws IOException {
        // Custom logic to return a JSON error response
        ApiResponse apiResponse = new ApiResponse(HttpStatus.FORBIDDEN, "Access denied");
        response.setStatus(apiResponse.getStatus().value());
        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        response.getWriter().write(apiResponse.toString());
    }
}
```

### 2. `@PreAuthorize` and `@PostAuthorize` Annotations
Spring Security provides annotations like `@PreAuthorize` and `@PostAuthorize` to handle method-level security. These annotations can be used to automatically detect unauthorized access and throw `AccessDeniedException` if necessary, saving you from manually checking access in your code:
```java
@PreAuthorize("hasRole('ROLE_ADMIN')")
public void deleteResource(Long resourceId) {
    // Delete the resource
}

@PostAuthorize("returnObject.owner == authentication.principal.username")
public Resource getResource(Long resourceId) {
    // Retrieve and return the resource
}
```

## Conclusion
Securing your Spring application is of utmost importance to protect sensitive data and regulate user access. Understanding and effectively handling the `AccessDeniedException` can ensure that unauthorized access attempts are appropriately handled, providing a robust security framework for your application.

In this article, we explored the `AccessDeniedException` in Spring, learned about different ways to handle it, and explored various customization options. By leveraging these capabilities, you can enhance the security of your Spring application with ease.

Take a step forward and integrate `AccessDeniedException` handling in your Spring application today! For more information, refer to the official Spring Security documentation: [https://docs.spring.io/spring-security/site/docs/current/reference/html5/](https://docs.spring.io/spring-security/site/docs/current/reference/html5/).

*Estimated reading time: 15 minutes*