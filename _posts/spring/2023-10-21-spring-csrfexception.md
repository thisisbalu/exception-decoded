---
title: "Understanding and Resolving CsrfException in Spring Framework: Your Ultimate Guide"
date: 2023-10-24 09:00:00 -0000
categories: [Spring, spring-security]
tags: [spring, spring-unchecked, org.springframework.security.web.server.csrf]
mermaid: true
toc: true
---

---


_As a developer, barriers in the form of exceptions can cut short our sprinting code. One such hurdle, the notorious CsrfException in the Spring Framework, presents its challenges. By better understanding and knowing how to resolve this exception, we can maintain a steady stride in creating efficient Spring applications._

---

Cross-Site Request Forgery (CSRF) is a form of attack that forces an end user to execute undesirable actions in a web application in which they're authenticated. If left unchecked, this can lead to unwanted and potentially dangerous effects. 

In this handy guide, we will dissect CsrfException and learn how to resolve this security concern in a Spring application.

## Understanding CsrfException in Spring

![Enter image description here](https://)
In the Spring Security ecosystem, CsrfException is thrown when a CSRF token is either missing or incorrect. This exception usually arises when you are working with forms and fail to include a CSRF token in the form data.

Here's an example of the exception message you may encounter:

```java
org.springframework.security.web.csrf.CsrfException: Missing or non-matching CSRF token.
```

## How Spring Security CSRF Protection Works

The Spring framework provides excellent built-in CSRF protection. Here's how it operates:

- For every state-changing request (POST, PUT, DELETE, PATCH), Spring Security includes a CSRF token.
- This CSRF token is automatically included as a parameter in your forms if you use Thymeleaf.
- With every state-changing request, Spring Security validates the CSRF token. If the token doesn't exist or doesn't match, a `CsrfException` is thrown.

Keep in mind that for Rest APIs, Spring Security by default does not implement CSRF protection.

## How to Resolve CsrfException

If you encounter a `CsrfException`, here are the steps to resolve it:

1. **Include CSRF Token**: Make sure to include a CSRF token in your form. If you're using Thymeleaf, the token is automatically included. If you're using JSP, you can manually include it with the following code segment:

    ```jsp
    <input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}"/>
    ```

2. **Disable CSRF**: If your application is a Rest API and doesn't have a GUI, disabling CSRF can be an option. Please note that this should only be a temporary solution, as it leaves your application vulnerable to CSRF attacks. Use the following code to disable CSRF:

    ```java
    @EnableWebSecurity
    public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http.csrf().disable();
        }
    }
    ```

3. **Handle CsrfException**: Spring allows you to handle CSRF failure, redirecting users to a specific URL when CsrfException occurs. This way, the application continues its functions without disruption. Below is the code snippet:

    ```java
    @EnableWebSecurity
    public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http.csrf().csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
                .and()
                .exceptionHandling()
                .authenticationEntryPoint(new HttpStatusEntryPoint(HttpStatus.UNAUTHORIZED));
        }
    }
    ```

By following these steps, you can effectively resolve CsrfException and secure your application against potential CSRF threats.

## Concluding Thoughts

Understanding and managing exceptions is crucial in application development, and CsrfException in Spring is no exception. By wrapping your head around how Spring Security's CSRF protection works and how to address CsrfException effectively, you are one step closer to producing a secure and efficient application with Spring.

We hope this guide has managed to throw some light on dealing with CsrfException in the Spring framework. Happy coding!

**Reference Links**

1. [csrf](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-csrf) in Spring Security Reference
2. [Spring Security — CSRF Protection](https://www.baeldung.com/spring-security-csrf)
3. [Spring MVC + Spring Security with pure Java Config](http://spring.io/blog/2013/07/04/spring-security-java-config-preview-csrf-protection/)

---

Let me know in the comments if you have any questions or if there's anything else about Spring Security you're interested in learning more about. If you found this guide useful, feel free to share it with your colleagues!
