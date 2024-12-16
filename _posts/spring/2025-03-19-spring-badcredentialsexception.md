---
title: "Understanding BadCredentialsException in Spring Security"
date: 2025-03-19 09:00:00 -0000
categories: [Spring, spring-security]
tags: [spring, spring-unchecked, org.springframework.security.authentication]
mermaid: true
toc: true
---


When it comes to securing your applications, Spring Security provides a robust framework for authentication and authorization. One common exception that developers encounter while implementing authentication mechanisms is the `BadCredentialsException`. This exception can cause confusion if not handled properly. In this article, we will dive into what `BadCredentialsException` is, when it occurs, and how to handle it effectively in your Spring applications.

## What is BadCredentialsException?

`BadCredentialsException` is a runtime exception in Spring Security that occurs during the authentication process. It indicates that a user has provided invalid credentials (such as a wrong username or password). This exception is part of the `org.springframework.security.authentication` package and extends the `AuthenticationException`, which is a parent class for all authentication-related exceptions.

Here’s a quick overview of when you might encounter `BadCredentialsException`:

- The username does not exist in the database.
- The password provided does not match the stored password.
- An expired or disabled account is attempting to authenticate.

## When Does BadCredentialsException Occur?

Let's take a look at a few scenarios that trigger the `BadCredentialsException`.

### Scenario 1: Incorrect Username or Password

When a user attempts to log in with a username that isn’t found in your user repository or provides a wrong password, the `BadCredentialsException` is thrown. This is the most common use case.

### Scenario 2: Disabled or Expired User Account

If an account has been disabled or expired and the user tries to log in, Spring Security will also throw a `BadCredentialsException`, although the specific exception is `DisabledException` or `AccountExpiredException`.

## Example of BadCredentialsException in Spring

Let’s see how `BadCredentialsException` can be used in a Spring Security configuration.

### Step 1: Spring Security Configuration

First, make sure to add the Spring Security dependency to your `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### Step 2: Custom Authentication Filter

Next, you can create a custom authentication filter that handles invalid credentials by catching `BadCredentialsException`.

```java
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class CustomAuthenticationFilter extends UsernamePasswordAuthenticationFilter {

    private final AuthenticationManager authenticationManager;

    public CustomAuthenticationFilter(AuthenticationManager authenticationManager) {
        this.authenticationManager = authenticationManager;
    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response) {
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        UsernamePasswordAuthenticationToken authToken = 
            new UsernamePasswordAuthenticationToken(username, password);

        try {
            return authenticationManager.authenticate(authToken);
        } catch (BadCredentialsException e) {
            throw new CustomBadCredentialsException("Invalid username or password");
        }
    }
}
```

### Step 3: Handling the Exception

You can create a controller advice to handle the `BadCredentialsException` and return appropriate error messages to the user.

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
        // You can log the exception or return a custom error response
        System.out.println(ex.getMessage());
    }
}
```

## Best Practices for Handling BadCredentialsException

1. **Custom Messages**: Always provide user-friendly error messages instead of exposing internal exceptions.
2. **Logging**: Log errors for monitoring and debugging purposes without compromising user data.
3. **Brute Force Protection**: Implement protective mechanisms against brute force attacks, such as rate limiting after several failed login attempts.
4. **User Feedback**: Provide direct feedback for login errors (e.g., "Invalid username or password") while avoiding specifics to protect against user enumeration attacks.

## Conclusion

`BadCredentialsException` is a vital part of error handling in Spring Security, especially during the authentication process. Understanding how to properly catch and respond to this exception can greatly enhance user experience and application security. By following best practices in handling this exception, you can ensure that your application not only responds accurately but also remains secure.

## References

- [Spring Security Documentation](https://docs.spring.io/spring-security/site/docs/current/reference/html5/)
- [Handling Authentication in Spring Security](https://www.baeldung.com/spring-security-authentication-and-access-control)
- [Custom Spring Security Authentication](https://www.baeldung.com/spring-security-authentication-provider)