---
title: "Title: AccountAlreadyClosedException in AWS Organizations: How to Handle and Troubleshoot"
date: 2024-03-20 09:00:00 -0000
categories: [AWS, AWS Organizations]
tags: [aws, organizations, com.amazonaws.services.organizations.model]
mermaid: true
toc: true
---


## Introduction

In AWS Organizations, the `AccountAlreadyClosedException` is an exception class that is thrown when attempting to close an AWS account that is already closed. This article provides a detailed overview of this exception class, its causes, and potential troubleshooting steps to resolve it. If you are an AWS Organizations user who encountered this exception, this comprehensive guide is here to assist you.

## Understanding AccountAlreadyClosedException

The `AccountAlreadyClosedException` is a class belonging to the `com.amazonaws.services.organizations.model` package in AWS SDK for Java. This exception occurs when calling the `CloseAccount` API in AWS Organizations for an account that is already closed.

## Causes of AccountAlreadyClosedException

There are a few possible causes for encountering this exception:

1. **Concurrency issues**: Multiple requests to close the same account simultaneously can result in this exception. This might happen when orchestrating account closure processes, especially if they are conducted in parallel.

2. **Incorrect account status**: It is possible that the account is already closed, but this information has not been updated accurately in the AWS Organizations service.

3. **Account closure in progress**: If an account closure process is already underway, attempting to close the account again will throw this exception. This typically happens when there is a delay in updating the account status across AWS Organizations.

## Troubleshooting AccountAlreadyClosedException

To troubleshoot the `AccountAlreadyClosedException` in AWS Organizations, you can follow these steps:

### Ensure Sequential Account Closures

If you are orchestrating the account closure process, ensure that the closure requests are executed sequentially instead of concurrently. This prevents multiple requests from closing the same account at the same time.

### Verify Account Closure Status

Use the AWS Management Console, CLI command, or AWS SDK to verify the current status of the account. If the account shows as already closed, the exception might be due to an outdated account status within AWS Organizations. In such cases, waiting for the status to be updated can resolve the issue.

### Retry After Waiting

In situations where multiple requests to close the same account have been made, waiting for some time before retrying can help avoid the `AccountAlreadyClosedException`. Delaying the subsequent closure request allows time for the previous closure request to complete and update the account status accurately.

### Contact AWS Support

If you have exhausted all troubleshooting options and the `AccountAlreadyClosedException` persists, reaching out to AWS Support can provide further assistance. They can investigate the issue and provide guidance specific to your AWS account.

## Code Examples

Here are a few code examples demonstrating how to handle the `AccountAlreadyClosedException` in your AWS SDK for Java implementation:

### Example 1: Handling AccountAlreadyClosedException

```java
try {
    organizationsClient.closeAccount(new CloseAccountRequest().withAccountId("123456789012"));
} catch (AccountAlreadyClosedException e) {
    // Handle exception, log error, or take appropriate action
    System.out.println("Account is already closed");
}
```

### Example 2: Verifying Account Closure Status

```java
DescribeAccountRequest describeAccountRequest = new DescribeAccountRequest().withAccountId("123456789012");
DescribeAccountResult describeAccountResult = organizationsClient.describeAccount(describeAccountRequest);
if (describeAccountResult.getAccount().getStatus() == accountStatus.CLOSED) {
    // Account is already closed, proceed accordingly
}
```

### Example 3: Retrying Request after Delay

```java
// Perform initial closure request
organizationsClient.closeAccount(new CloseAccountRequest().withAccountId("123456789012"));

// Delay subsequent closure requests
Thread.sleep(5000); // 5 seconds

// Retry closure request
organizationsClient.closeAccount(new CloseAccountRequest().withAccountId("123456789012"));
```

## Conclusion

By understanding the `AccountAlreadyClosedException` and following the troubleshooting steps mentioned in this article, you can effectively handle and troubleshoot this exception in AWS Organizations. Remember to ensure sequential closure requests, verify account status, and retry after waiting to eliminate this exception. Don't hesitate to contact AWS Support for further assistance if needed.

For more information about the `AccountAlreadyClosedException` and AWS Organizations, refer to the following references:

- [AWS SDK for Java API Reference](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/)
- [AWS Organizations Developer Guide](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_reference.html)

**Note**: This article provides general guidance and steps for troubleshooting the `AccountAlreadyClosedException`. It is recommended to refer to the official AWS documentation and consult with AWS Support for specific issues and account-related concerns.

*Estimated reading time: 15 minutes*