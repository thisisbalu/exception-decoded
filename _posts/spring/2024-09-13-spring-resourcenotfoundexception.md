---
title: "**Handling ResourceNotFoundException in Spring**"
date: 2024-09-13 09:00:00 -0000
categories: [Spring, spring-data]
tags: [spring, spring-unchecked, org.springframework.data.rest.webmvc]
mermaid: true
toc: true
---


## Introduction

In any Spring application, handling exceptions is crucial for providing a meaningful response to the users. One common exception that developers often encounter is the `ResourceNotFoundException`. This article will explore in detail what this exception is and how to handle it effectively in a Spring application.

## What is ResourceNotFoundException?

The `ResourceNotFoundException` is an exception class provided by the Spring framework. It is typically thrown when a requested resource is not found. Resources can include database records, files, URLs, or any other type of data that the application interacts with. 

This exception belongs to the family of RuntimeExceptions, which means it does not require explicit handling. However, properly handling this exception can greatly enhance the user experience and provide valuable feedback.

## Why should we handle ResourceNotFoundException?

When an application faces a `ResourceNotFoundException`, it means that a requested resource (e.g., a user record, a file, an image, etc.) could not be found. By handling this exception, we have an opportunity to gracefully respond to the user by providing a helpful error message or even redirecting them to a relevant page.

By not handling this exception, the application may throw a generic error message, leaving the users frustrated and confused. Additionally, for better SEO, it is essential to handle exceptions properly to avoid error pages with a high error code rate.

## Handling ResourceNotFoundException in Spring

To effectively handle the `ResourceNotFoundException` in a Spring application, we can leverage the custom error handling mechanism provided by Spring MVC. By extending the `ResponseEntityExceptionHandler` class and overriding its methods, we can customize error responses based on specific exceptions.

Let's dive into an example:

```java
@ControllerAdvice
public class CustomExceptionHandler extends ResponseEntityExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorDetails> handleResourceNotFoundException(
            ResourceNotFoundException ex, WebRequest request) {
       
        ErrorDetails errorDetails = new ErrorDetails(new Date(), ex.getMessage(), request.getDescription(false));
        
        return new ResponseEntity<>(errorDetails, HttpStatus.NOT_FOUND);
    }
}
```

In the code snippet above, we define a custom exception handler by using the `@ControllerAdvice` annotation. This class extends `ResponseEntityExceptionHandler` and overrides the `handleResourceNotFoundException` method, which handles the `ResourceNotFoundException`.

The overridden method builds an `ErrorDetails` object containing the error message, timestamp, and a description of the request. This object is then returned within a `ResponseEntity` with an appropriate HTTP status code (in this case, `HttpStatus.NOT_FOUND`).

## Usage of the CustomExceptionHandler 

To enable the custom exception handling in our Spring application, we need to include the following bean configuration:

```java
@Configuration
public class CustomExceptionHandlerConfig implements WebMvcConfigurer {

    @Bean
    public CustomExceptionHandler customExceptionHandler() {
        return new CustomExceptionHandler();
    }

    @Override
    public void configureHandlerExceptionResolvers(
            List<HandlerExceptionResolver> resolvers) {
        resolvers.add(customExceptionHandler());
    }
}
```

Here, we define a `WebMvcConfigurer` bean that registers our `CustomExceptionHandler` as a resolver for handling exceptions.

## Testing the Exception Handling

To simulate the `ResourceNotFoundException` scenario, we can create a simple REST controller that attempts to retrieve a non-existent resource:

```java
@RestController
public class UserController {

    // User Service
    @Autowired
    private UserService userService;

    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) throws ResourceNotFoundException {
        User user = userService.getUserById(id);
        if (user == null) {
            throw new ResourceNotFoundException("User not found with id: " + id);
        }
        return ResponseEntity.ok(user);
    }
}
```

In this example, if the requested user does not exist, we throw the `ResourceNotFoundException`, passing a custom error message.

## Conclusion

Handling the `ResourceNotFoundException` is a crucial part of building Spring applications that provide a smooth user experience. By implementing a custom exception handler, we can enrich the error response with meaningful information to assist users and improve the application's SEO.

In this article, we explored how to create a custom exception handler using `ResponseEntityExceptionHandler` and showcased an example of handling the `ResourceNotFoundException` in a REST controller. Remember to handle exceptions with care, as it greatly affects the overall usability and satisfaction of your application.

For more information and best practices on exception handling in Spring, check out the official Spring documentation: 
[Spring Exception Handling Guide](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-rest-spring-mvc-exceptions).

Happy coding!