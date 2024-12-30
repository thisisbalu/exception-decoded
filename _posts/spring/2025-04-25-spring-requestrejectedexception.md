---
title: "Understanding RequestRejectedException in Spring Framework"
date: 2025-04-25 09:00:00 -0000
categories: [Spring, spring-security]
tags: [spring, spring-unchecked, org.springframework.security.web.firewall]
mermaid: true
toc: true
---


In the world of Spring Framework, handling exceptions efficiently is crucial for building robust applications. Among many exceptions that developers encounter, `RequestRejectedException` stands out as an important one specifically when dealing with servlet requests. In this article, we will explore what `RequestRejectedException` is, when it occurs, and how to handle it effectively in your Spring applications. 

## What is RequestRejectedException?

`RequestRejectedException` is a runtime exception provided by the Spring Framework, primarily in the context of web applications. This exception is thrown by the Spring MVC framework when a request cannot be processed for security reasons or because it is malformed. This mechanism helps prevent issues like HTTP Request smuggling and protects against other types of malformed requests.

### Common Causes of RequestRejectedException

1. **Invalid Host Header**: If the incoming request contains an invalid or unexpected `Host` header, Spring may reject it.
  
2. **HTTP Method Not Allowed**: When a client attempts to send a request with a method (like PUT or DELETE) that is not allowed by a specific endpoint, the request may be rejected.

3. **Malformed URL**: Requests with illegal characters in the URL, such as spaces or control characters, can trigger this exception.

4. **Excessive Headers**: A request with an unusually high number of headers can also be rejected for performance or security reasons.

5. **Blocked IPs**: If your application has specific access control rules, requests from certain IP addresses may result in a `RequestRejectedException`.

### Example of When Exception Occurs

Let’s consider a simple Spring Boot application that has a REST controller. 

```java
@RestController
@RequestMapping("/api")
public class MyController {

    @GetMapping("/hello")
    public ResponseEntity<String> hello() {
        return ResponseEntity.ok("Hello, World!");
    }
}
```

If a request is made to the `/api/hello` endpoint with an invalid Host header, Spring will throw a `RequestRejectedException`.

## Handling RequestRejectedException

Handling such exceptions gracefully is essential to enhance user experience while ensuring security. You can create a global exception handler using `@ControllerAdvice` and manage the `RequestRejectedException` like so:

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;
import org.springframework.web.util.RequestRejectedException;

@ControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {

    @ExceptionHandler(RequestRejectedException.class)
    public ResponseEntity<String> handleRequestRejectedException(RequestRejectedException ex) {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST)
                .body("Request was rejected: " + ex.getMessage());
    }
}
```

### Configuring the Tomcat Server for Request Limits

In Spring Boot applications, you might want to configure the embedded Tomcat server settings to control the request size limits. You can adjust the settings in `application.properties`:

```properties
server.tomcat.max-http-header-size=8192
server.tomcat.max-swallow-size=2097152
```

This configuration ensures that the Tomcat server does not reject requests that exceed certain limits.

## Debugging RequestRejectedException

When debugging an application, it may be helpful to log the exception details. You can use a custom filter to log relevant request information when a `RequestRejectedException` occurs:

```java
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class RequestLoggingFilter implements Filter {

    private static final Logger logger = LoggerFactory.getLogger(RequestLoggingFilter.class);

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
           throws IOException, ServletException {
        try {
            chain.doFilter(request, response);
        } catch (RequestRejectedException e) {
            logger.error("Request was rejected: {}", e.getMessage());
            throw e;
        }
    }

    @Override
    public void init(FilterConfig filterConfig) {}

    @Override
    public void destroy() {}
}
```

You can register the filter in your Spring Boot application:

```java
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FilterConfig {

    @Bean
    public FilterRegistrationBean<RequestLoggingFilter> loggingFilter(){
        FilterRegistrationBean<RequestLoggingFilter> registrationBean = 
          new FilterRegistrationBean<>(new RequestLoggingFilter());
          
        registrationBean.addUrlPatterns("/api/*"); // Adjust as needed

        return registrationBean;    
    }
}
```

## Best Practices for Avoiding RequestRejectedException

To avoid running into `RequestRejectedException`, consider the following best practices:

1. **Validate Input**: Ensure all inputs are validated on the client-side before sending requests.

2. **Configure Security Filters**: Use Spring Security to manage security concerns and restrict invalid requests.

3. **Set Appropriate Limits**: Use `.properties` or `.yml` files effectively to configure limits on header sizes, request sizes, etc.

4. **Logging**: Employ logging mechanisms to capture information whenever a request is rejected, enabling easier debugging in the future.

5. **Testing**: Regularly test your endpoints with various scenarios including malformed requests to ensure your application behaves as expected.

## Conclusion

The `RequestRejectedException` is a vital exception in the Spring Framework that aids in maintaining the security and integrity of web applications. Understanding its causes, implementing proper handling mechanisms, and following best practices can significantly enhance the user experience and security of your applications.

For further reading on Spring exception handling, consider checking the official documentation and other references below:

## References

- [Spring Framework Documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-exception-handling)
- [RequestRejectedException on GitHub](https://github.com/spring-projects/spring-framework/issues/25100)
- [Spring Boot Reference Documentation](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/) 
- [Understanding Servlet Filters](https://docs.oracle.com/javaee/7/api/javax/servlet/Filter.html) 

By leveraging the knowledge of `RequestRejectedException`, you can build secure, reliable, and user-friendly Spring applications.
