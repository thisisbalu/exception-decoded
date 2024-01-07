---
title: "Title: EmptyResultDataAccessException in Spring: Handling Empty Results with Grace"
date: 2024-03-07 09:00:00 -0000
categories: [Spring, spring-framework]
tags: [spring, spring-unchecked, org.springframework.dao]
mermaid: true
toc: true
---


## Introduction
In any Spring application, data retrieval plays a crucial role. However, there are instances where we expect a result from a database query or a data service call, but end up with an empty result set. To effectively handle this situation, Spring provides the `EmptyResultDataAccessException` exception. In this article, we will explore how to use this exception to gracefully handle empty results in a Spring application.

## Understanding EmptyResultDataAccessException
The `EmptyResultDataAccessException` is a runtime exception provided by the Spring Framework. It is thrown when we expect a single result or a single row from a query, but the result set turns out to be empty. This exception extends the `DataAccessException` class, which is the parent class for all Spring-specific database-related exceptions.

## Typical Scenario
Consider a scenario where we need to retrieve a user's details based on their unique identifier. We create a method in our Spring Data repository to find a user by their ID:

```java
public interface UserRepository extends JpaRepository<User, Long> {
    User findUserById(Long id);
}
```

In our service class, we invoke this method to retrieve the user by their ID:

```java
@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User getUserById(Long id) {
        return userRepository.findUserById(id);
    }
}
```

Now, when we call the `getUserById` method with a valid ID that doesn't exist in the database, it will throw an `EmptyResultDataAccessException` with a meaningful error message.

### Handling EmptyResultDataAccessException
Spring recommends handling the `EmptyResultDataAccessException` in our code to provide a graceful response to empty result sets. Let's walk through a few examples of how we can achieve this.

##### Example 1: Returning a Default Value
In some cases, we may want to return a default value instead of raising an exception when a query returns no results. For instance, if we can't find a user by their ID, we can return a default user with empty details. To achieve this, we can modify our service code as follows:

```java
public User getUserById(Long id) {
    try {
        return userRepository.findUserById(id);
    } catch (EmptyResultDataAccessException ex) {
        return new User(); // Return a default user
    }
}
```

By catching the `EmptyResultDataAccessException` and returning a default user object, we ensure our code doesn't throw an exception when there are no search results.

##### Example 2: Responding with Custom Error Views
Sometimes, we may want to display a custom error message or view when encountering an empty result set. We can leverage Spring's exception handling mechanism to achieve this. First, we create an exception handler method in our controller:

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(EmptyResultDataAccessException.class)
    public ModelAndView handleEmptyResultException(EmptyResultDataAccessException ex) {
        ModelAndView modelAndView = new ModelAndView("errorView");
        modelAndView.addObject("errorMessage", "No results found.");

        return modelAndView;
    }
}
```

In the above code, we use the `@ExceptionHandler` annotation to handle the `EmptyResultDataAccessException` and map it to our custom error view. The created `ModelAndView` object allows us to pass additional attributes, such as an error message, to the error view. Finally, we define our error view template, `errorView.html`, to display the error message to the user.

##### Example 3: Throwing a Custom Exception
In certain scenarios, we may want to throw a custom exception instead of the `EmptyResultDataAccessException`. We can accomplish this by creating a custom exception class and leveraging Spring's exception translation mechanism.

First, we define our custom exception:

```java
public class UserNotFoundException extends RuntimeException {
    // Custom exception code
}
```

Next, we configure Spring to translate the `EmptyResultDataAccessException` into our custom exception:

```java
@Configuration
public class DataAccessExceptionTranslator implements PersistenceExceptionTranslator {

    @Autowired
    private EmptyResultDataAccessExceptionTranslator exceptionTranslator;

    @Override
    public DataAccessException translateExceptionIfPossible(RuntimeException ex) {
        if (ex instanceof EmptyResultDataAccessException) {
            throw new UserNotFoundException();
        }
        return exceptionTranslator.translateExceptionIfPossible(ex);
    }
}
```

In the above code, we implement the `PersistenceExceptionTranslator` interface to translate exceptions thrown by the underlying data access code. When the `EmptyResultDataAccessException` occurs, our custom exception `UserNotFoundException` is thrown instead.

## Conclusion
In this article, we explored the `EmptyResultDataAccessException` in Spring, which helps us handle empty results with simplicity and elegance. We saw different approaches to gracefully handle empty result sets, such as returning default values, displaying custom error views, and throwing custom exceptions.

By using the `EmptyResultDataAccessException` effectively, we can enhance the user experience and prevent unexpected application behavior when dealing with empty result sets in Spring.

## References
- [Spring Framework Documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#dao-exceptions)
