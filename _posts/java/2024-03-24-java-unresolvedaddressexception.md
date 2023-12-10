---
title: "UnresolvedAddressException in Java: Explained with Code Examples"
date: 2024-03-24 09:00:00 -0000
categories: [Java, java.base]
tags: [java, java-unchecked, java.nio.channels, java-se]
mermaid: true
toc: true
---


Are you encountering an `UnresolvedAddressException` while working with Java network programming? Don't worry, you're not alone! This common exception occurs when attempting to establish a connection with a remote server and the address cannot be resolved. In this article, we'll dive deep into the causes, solutions, and code examples to help you better understand and handle this exception efficiently.

## Understanding UnresolvedAddressException

The `UnresolvedAddressException` is a subclass of `IOException` that is thrown when a socket address cannot be resolved to a valid address. This typically occurs due to one of the following reasons:

1. **Hostname or IP address does not exist:** If the hostname or IP address you are trying to connect to is invalid or non-existent, the `UnresolvedAddressException` will be thrown.

2. **DNS resolution failure:** This exception may also occur if the Domain Name System (DNS) fails to resolve the hostname to an IP address.

## Common Situations Leading to UnresolvedAddressException

Let's look at some common scenarios that can trigger an `UnresolvedAddressException`.

### 1. Invalid Hostname or IP Address

Attempting to establish a connection using an invalid or non-existent hostname or IP address will result in an `UnresolvedAddressException`. Let's consider the following code snippet:

```java
try {
    InetAddress address = InetAddress.getByName("invalidhostname");
    Socket socket = new Socket(address, 8080);
} catch (UnresolvedAddressException e) {
    System.out.println("Invalid hostname or IP address");
    e.printStackTrace();
} catch (IOException e) {
    // Handle other IO exceptions
}
```

In this example, we're trying to create a socket connection to the hostname "invalidhostname" on port 8080. Since this hostname doesn't exist, the `getByName()` method will throw an `UnresolvedAddressException`.

### 2. DNS Resolution Failure

Another common scenario is DNS resolution failure. If the DNS fails to resolve the hostname to an IP address, an `UnresolvedAddressException` will be thrown. Consider the following code snippet:

```java
try {
    InetAddress address = InetAddress.getByName("example.com");
    Socket socket = new Socket(address, 8080);
} catch (UnresolvedAddressException e) {
    System.out.println("DNS resolution failed");
    e.printStackTrace();
} catch (IOException e) {
    // Handle other IO exceptions
}
```

In this example, if the DNS fails to resolve the hostname "example.com" to a valid IP address, the `getByName()` method will throw an `UnresolvedAddressException`.

## Handling the UnresolvedAddressException

To handle the UnresolvedAddressException, it's crucial to identify the root cause and take appropriate actions. Here are a few strategies you can apply:

### 1. Validating the Hostname or IP Address

Before establishing a connection, ensure that the hostname or IP address you are using is valid and exists. You can use regular expressions or a library like Apache Commons Validator to validate the address.

```java
import org.apache.commons.validator.routines.InetAddressValidator;

String hostname = "invalidhostname";
InetAddressValidator validator = InetAddressValidator.getInstance();
if (validator.isValid(hostname)) {
    InetAddress address = InetAddress.getByName(hostname);
    Socket socket = new Socket(address, 8080);
} else {
    System.out.println("Invalid hostname or IP address");
}
```

By validating the hostname or IP address before creating the socket connection, you can prevent the `UnresolvedAddressException`.

### 2. Resolving DNS using InetAddress getAllByName

To avoid DNS resolution failure, you can use the `InetAddress.getAllByName()` method, which returns an array of all IP addresses associated with a hostname.

```java
String hostname = "example.com";
InetAddress[] addresses = InetAddress.getAllByName(hostname);
Socket socket = new Socket(addresses[0], 8080);
```

In this example, we're obtaining all IP addresses associated with the hostname "example.com" and then creating a socket connection using the first address from the array.

### 3. Handling Exceptions and Retry

If the initial connection attempt fails with an `UnresolvedAddressException`, you can catch the exception and retry after a certain delay. This strategy allows the DNS resolution to potentially succeed in subsequent attempts.

```java
String hostname = "example.com";
int retries = 3;
int delay = 1000; // milliseconds

for (int i = 0; i < retries; i++) {
    try {
        InetAddress address = InetAddress.getByName(hostname);
        Socket socket = new Socket(address, 8080);
        break;
    } catch (UnresolvedAddressException e) {
        System.out.println("DNS resolution failed. Retrying...");
        Thread.sleep(delay);
    } catch (IOException e) {
        // Handle other IO exceptions
    }
}
```

In this example, we're retrying the DNS resolution up to three times with a delay of one second between each attempt.

## Conclusion

In this article, we explored the `UnresolvedAddressException` in Java network programming. We discussed various causes leading to this exception, including invalid hostnames, non-existent IP addresses, and DNS resolution failures. Moreover, we presented several code examples to help you handle this exception effectively.

By validating the hostname or IP address, resolving DNS using `InetAddress.getAllByName()`, or implementing a retry mechanism, you can resolve the `UnresolvedAddressException` and ensure smoother network connections in your Java applications.

Keep in mind that by following best practices and error handling techniques mentioned in this article, you can handle the `UnresolvedAddressException` more efficiently and improve the overall reliability of your networking code.

For more information and detailed examples, please refer to the following references:

- [Java Documentation: UnresolvedAddressException](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/UnresolvedAddressException.html)
- [Apache Commons Validator](https://commons.apache.org/proper/commons-validator/)
- [DNS Resolution in Java](https://www.baeldung.com/java-dns-resolution)

Happy coding!
