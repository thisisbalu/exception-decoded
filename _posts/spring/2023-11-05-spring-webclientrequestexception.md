---
title: "Under the Hood with WebClientRequestException in Spring WebFlux"
date: 2023-11-27 09:00:00 -0000
categories: [Spring, spring-framework]
tags: [spring, spring-unchecked, org.springframework.web.reactive.function.client]
mermaid: true
toc: true
---


As you dive deeper into sprawling world of **Spring WebFlux**, you're bound to come across some hurdles that might initially seem daunting. One such nuisance is the `WebClientRequestException` that pops up and threatens to slow down your development pace. But don't fret, as we're here to equip you with the tools to navigate these choppy waters effectively. 

This blog will dive deep into what `WebClientRequestException` really means and how you can efficiently manage this exception in the context of _Spring WebFlux_ for your web application development needs.

> Content Summary:
1. [Spring WebFlux Overview](#spring-webflux-overview)
2. [Understanding WebClientRequestException](#understanding-webclientrequestexception)
3. [Handling WebClientRequestException](#handling-webclientrequestexception)
4. [Relevant Code Examples](#relevant-code-examples)
5. [Tests Weaving WebClientRequestException](#tests-weaving-webclientrequestexception)
   
## Spring WebFlux Overview

In essence, Spring WebFlux is a reactive web framework – part of Spring 5.0 – built to take on asynchronous non-blocking operations in applications. It offers forth a powerful model for developing applications with [highly concurrent programming](https://en.wikipedia.org/wiki/Concurrent_computing), meeting the demands of mushrooming web and enterprise applications.

```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

You can add the above dependency in your `pom.xml` to start using Spring WebFlux.

## Understanding WebClientRequestException

`WebClientRequestException` in Spring WebFlux shows up when an error occurs while sending requests to the server. It is a subtype of `WebClientResponseException`. The exception is typically thrown due to issues related to connectivity, such as a malfunctioning network or server. 

```java
//Create an instance of WebClient.
WebClient webclient = WebClient.create();

Mono<String> result = webClient
    .get()
    .uri("https://nonexisting.server.com") // Server/URL not accessible
    .retrieve()
    .bodyToMono(String.class);

// Handle WebClientRequestException
result.subscribe(
    value -> System.out.println(value),
    error -> System.err.println("Error: " + error.printStackTrace())
);
```

In the above code, `retrieve()` throws `WebClientRequestException` when the server is not reachable.

## Handling WebClientRequestException

Handling WebClientRequestException requires careful understanding of the nature of the exception. To handle `WebClientRequestException`, try to diagnose the stack trace it provides -which can help identify the root cause of the issue. You may also consider using `onError` operator.

```java
String result = webClient
    .get()
    .uri("https://non-existing.server.com")
    .retrieve()
    .bodyToMono(String.class)
    .onErrorResume(WebClientRequestException.class, ex -> {
        System.err.println("Connection error: " + ex.getMessage());
        return Mono.just("Fallback");
    })
    .block();
```

## Relevant Code Examples

WebClientRequestException can be handled in different ways. An alternative way to handle these errors is to use the `doOnError()` function, as shown below:

```java
Mono<String> stringMono = webClient
    .get()
    .uri("http://non-existing.server.com")
    .retrieve()
    .bodyToMono(String.class)
    .doOnError(WebClientRequestException.class, e -> {
        System.err.println("Connection error: " + e.getMessage());
    });

StepVerifier.create(stringMono)
    .verifyErrorMessage("Connection error");
```

## Tests Weaving WebClientRequestException

Tests need to cater to situations where WebClientRequestException might be thrown. 

```java
@Test
public void handleWebClientRequestExceptionTest() {
    webClient
        .get()
        .uri("http://non-existing.server.com")
        .retrieve()
        .bodyToMono(String.class)
        .doOnError(WebClientRequestException.class, e -> {
            assertEquals("Connection error", e.getMessage());
        }).subscribe();
}
```

### Conclusion 
To summarize, understanding WebClientRequestException is crucial while working with Spring WebFlux and handling them correctly can help ensure the robustness of your application. By understanding, capturing, and handling these exceptions, you ensure that your app remains responsive.

### References
1. [Spring WebFlux Documentation](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html)
2. [Spring WebFlux Error Handling Guide](https://www.baeldung.com/spring-webflux-errors)
3. [WebClientRequestException javadocs](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/reactive/function/client/WebClientRequestException.html)
4. [Reactive Programming with Spring WebFlux](https://www.callicoder.com/spring-5-reactive-webclient-webtestclient-examples/) 
5. [Reactive Streams](https://www.reactive-streams.org/)