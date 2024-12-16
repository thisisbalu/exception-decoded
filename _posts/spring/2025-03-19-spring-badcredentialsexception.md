---
title: "Understanding BadCredentialsException in Spring Security"
date: 2025-03-19 09:00:00 -0000
categories: [Spring, spring-security]
tags: [spring, spring-unchecked, org.springframework.security.authentication]
mermaid: true
toc: true
---


In the realm of Spring Security, one of the most common exceptions developers encounter is `BadCredentialsException`. This exception plays a crucial role in the authentication process, serving as a security measure to ensure that users provide valid credentials. In this article, we'll explore what `BadCredentialsException` is, its purpose, how to handle it effectively, and provide some practical examples to help deepen your understanding.

## What is BadCredentialsException?

`BadCredentialsException` is a runtime exception that extends from `AuthenticationException` within the Spring Security framework. It is thrown when an authentication attempt fails due to invalid credentials. This can occur during various authentication methods such as basic authentication, form-based login, or token-based systems.

### Common Scenarios for BadCredentialsException

1. **Wrong Username/Password**: The most frequent cause where users input incorrect login details.
2. **Account Lock**: In cases where an account may be temporarily locked due to multiple failed attempts.
3. **Expired Credentials**: If a user's credentials are expired or revoked.

## Basic Usage of Spring Security

Before diving deep into `BadCredentialsException`, let's set up a simple Spring Boot application with Spring Security in place.

### Setting Up Spring Security

We’ll create a basic Spring Boot application and configure Spring Security to understand how `BadCredentialsException` is triggered.

1. **Add Dependencies**: Add the following Maven dependency in your `pom.xml`.

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-security</artifactId>
   </dependency>
   ```

2. **Basic Security Configuration**: Create a configuration class for securing your application.

   ```java
   import org.springframework.context.annotation.Configuration;
   import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
   import org.springframework.security.config.annotation.web.builders.HttpSecurity;
   import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
   import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

   @Configuration
   @EnableWebSecurity
   public class SecurityConfig extends WebSecurityConfigurerAdapter {

       @Override
       protected void configure(AuthenticationManagerBuilder auth) throws Exception {
           auth.inMemoryAuthentication()
               .withUser("user").password("{noop}password").roles("USER")
               .and()
               .withUser("admin").password("{noop}admin").roles("ADMIN");
       }

       @Override
       protected void configure(HttpSecurity http) throws Exception {
           http
               .authorizeRequests()
               .anyRequest().authenticated()
               .and()
               .formLogin()
               .permitAll()
               .and()
               .logout().permitAll();
       }
   }
   ```

### Generating a BadCredentialsException

Once your security configuration is ready, you can run your application. Attempt to log in with the wrong credentials to see how `BadCredentialsException` is thrown:

1. Start your Spring Boot application.
2. Visit `http://localhost:8080` and enter invalid credentials like `username: wronguser` and `password: wrongpass`.

You will be greeted with a login error page indicating that the username or password is incorrect.

## Custom Error Handling for BadCredentialsException

Handling `BadCredentialsException` gracefully is crucial for user experience. You can customize how your application reacts when this exception is thrown.

### Implementing a Custom Authentication Failure Handler

Spring Security allows you to define a custom failure handler. This can be done by implementing the `AuthenticationFailureHandler` interface.

Here’s how you can do it:

```java
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class CustomAuthenticationFailureHandler implements AuthenticationFailureHandler {

    @Override
    public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response,
                                        AuthenticationException exception) throws IOException {
        if (exception instanceof BadCredentialsException) {
            response.sendRedirect("/login?error=bad_credentials");
        } else {
            response.sendRedirect("/login?error=other");
        }
    }
}
```

### Registering Your Custom Handler

Replace the default login configuration in your `SecurityConfig`:

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .authorizeRequests()
        .anyRequest().authenticated()
        .and()
        .formLogin()
        .failureHandler(new CustomAuthenticationFailureHandler())
        .permitAll()
        .and()
        .logout().permitAll();
}
```

With this implementation, incorrect attempts will redirect the user to a login page with a specific error message. You can further improve the error handling by providing custom messages.

## Logging BadCredentialsException

For security auditing, it is also essential to log authentication failures. You can use Spring’s `@ControllerAdvice` to globally handle exceptions across your application.

### Example of Logging the Exception

First, create a global exception handler:

```java
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseStatus;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(BadCredentialsException.class)
    @ResponseStatus(HttpStatus.UNAUTHORIZED)
    public void handleBadCredentials(BadCredentialsException ex) {
        // Log the exception to console or your logging framework of choice
        System.out.println("Bad Credentials Exception: " + ex.getMessage());
    }
}
```

This ensures that every occurrence of `BadCredentialsException` is logged, keeping track of authentication failures over time.

## Conclusion

`BadCredentialsException` is a vital part of Spring Security, enhancing your application's security by preventing unauthorized access. By understanding its purpose and implementing custom error handling and logging strategies, you ensure a better user experience and stronger security for your applications.

Using the strategies outlined in this article—such as custom failure handlers and global exception handling—enables you to handle authentication issues gracefully while maintaining the integrity of your application's security.

## References

- [Spring Security Documentation](https://docs.spring.io/spring-security/site/docs/current/reference/html5/)
- [BadCredentialsException Documentation](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/authentication/BadCredentialsException.html)
- [Spring Boot Official Guide](https://spring.io/guides/gs/spring-boot/)