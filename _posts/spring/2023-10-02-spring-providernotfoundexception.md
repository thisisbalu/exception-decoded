---
title: "Unraveling Spring Boot: Deconstructing the ProviderNotFoundException"
date: 2023-10-02 16:32:37 -0000
categories: [Spring, spring-security]
tags: [spring, spring-unchecked, org.springframework.security.authentication]
mermaid: true
toc: true
---

---
title: "Unraveling Spring Boot: Deconstructing the ProviderNotFoundException"
keywords: "Spring Boot, ProviderNotFoundException, Exception Handling, Spring Security, Spring Boot Tutorial"
---


Hello Spring Boot enthusiasts! In today's comprehensive guide, we delve into the lesser-known yet frequently encountered troublemaker in the Spring Boot universe - the `ProviderNotFoundException`.

Often, when developing applications using Spring Boot or Spring Security, you might face the `ProviderNotFoundException`. This exception is usually thrown when your software tries to access a specific provider that is not available in the application context. Don't worry! We are here to help you handle this exception and show you how to avoid it in the future.

## What is ProviderNotFoundException?

Let's start by understanding the exception. In Spring framework, `ProviderNotFoundException` is an exception that inherits from Spring's `AuthenticationException`. This exception is thrown when the `ProviderManager` class couldn't find a provider that could authenticate the presented `Authentication` object.

```java
public class ProviderNotFoundException extends AuthenticationException {
    public ProviderNotFoundException(String msg) {
       super(msg);
    }
}
```

## Why Does ProviderNotFoundException Occur?

The `ProviderNotFoundException` typically occurs when we have multiple authentication providers defined in our Spring Security configuration, and none of those providers is able to process the submitted `Authentication` token.

For instance, consider this scenario: You have a `DaoAuthenticationProvider` in your application context configured for `UsernamePasswordAuthenticationToken`. But then you send an authentication request using a `RememberMeAuthenticationToken`. The system will throw a `ProviderNotFoundException` because no available provider can handle the `RememberMeAuthenticationToken`.

## Code Example Demonstrating the Exception

Here's a simple example demonstrating this situation. Consider the following configuration:

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
  @Autowired
  private DataSource dataSource;

  @Override
  public void configure(AuthenticationManagerBuilder auth) throws Exception {  
    auth.jdbcAuthentication().dataSource(dataSource);
  }
}
```

Now, if you initiate a `RememberMeAuthenticationToken` in this context, you will encounter the `ProviderNotFoundException` because the `AuthenticationManager` cannot find an appropriate provider to process this type of authentication.

```java
RememberMeAuthenticationToken remToken = new RememberMeAuthenticationToken("someKey", "user",  AuthorityUtils.createAuthorityList("ROLE_USER"));
SecurityContextHolder.getContext().setAuthentication(remToken);
```

## How to Solve the ProviderNotFoundException?

Well, the resolution to this is quite straightforward. You just need to ensure that a matching `AuthenticationProvider` is present and configured in your application context for every type of authentication token used in your application. For instance, if you are using a `RememberMeAuthenticationToken`, make sure that you have a `RememberMeAuthenticationProvider` configured in your security context.

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
  @Autowired
  private DataSource dataSource;

  @Override
  protected void configure(HttpSecurity http) throws Exception {  
      http.rememberMe().key("someKey");
  }

  @Override
  public void configure(AuthenticationManagerBuilder auth) throws Exception {  
      auth.jdbcAuthentication().dataSource(dataSource);
      auth.authenticationProvider(rememberMeAuthenticationProvider());
  }

  @Bean
  public RememberMeAuthenticationProvider rememberMeAuthenticationProvider() {
      return new RememberMeAuthenticationProvider("someKey");
  }
}
```

In the above code, a `RememberMeAuthenticationProvider` is configured to handle `RememberMeAuthenticationToken`.

## Wrapping Up

The `ProviderNotFoundException` is a common hurdle faced by many while using Spring Security. However, once you understand why it occurs and how to handle it, it should be a breeze controlling its appearance in the future.

Keep this guide handy the next time you find yourself dealing with a `ProviderNotFoundException` and remember to configure the appropriate Providers for every Authentication token your application uses.

For more details on Spring Boot and its exceptions, you can refer to its [official documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/).

---

Happy Coding!
