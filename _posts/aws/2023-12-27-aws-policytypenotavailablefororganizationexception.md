---
title: "Exception Handling in AWS Organizations: PolicyTypeNotAvailableForOrganizationException"
date: 2023-12-27 09:00:00 -0000
categories: [AWS, AWS Organizations]
tags: [aws, organizations, com.amazonaws.services.organizations.model]
mermaid: true
toc: true
---

As organizations grow and expand their infrastructure on the cloud, managing user access and policies becomes crucial for effective governance and security. AWS Organizations is a powerful service that helps manage and govern multiple AWS accounts under a single umbrella. With its robust features, it enables centralized control over policies, service control policies, and enables you to automate the creation and management of AWS accounts.

However, while working with AWS Organizations, you might encounter various exceptions that need to be handled gracefully to ensure smooth operations. One such exception is the `PolicyTypeNotAvailableForOrganizationException` of `com.amazonaws.services.organizations.model`. This exception occurs when you attempt to create or update a policy type that is not available for use within your organization.

In this article, we will explore the details of the `PolicyTypeNotAvailableForOrganizationException` and how to handle it effectively using the AWS SDK for Java.

## Overview of the PolicyTypeNotAvailableForOrganizationException

The `PolicyTypeNotAvailableForOrganizationException` is a specific exception class available in the AWS SDK for Java when working with the AWS Organizations service. It indicates that the requested policy type is not available for use within the organization. This exception is typically thrown when you attempt to create or update a policy with a policy type that is unsupported or not enabled for your organization.

It is essential to understand the concept of policy types in AWS Organizations. Policy types define the structure and behavior of policies associated with the organization. AWS provides various built-in policy types such as `SERVICE_CONTROL_POLICY`, `TAG_POLICY`, and `AISERVICES_OPT_OUT_POLICY`. While these built-in policy types are available by default, it is possible to create custom policy types as well. However, not all policy types are compatible or supported by AWS Organizations, and this is where the exception `PolicyTypeNotAvailableForOrganizationException` comes into play.

## Understanding the Exception Signature

To effectively handle the `PolicyTypeNotAvailableForOrganizationException`, it is crucial to understand its constructor and its parameters. The constructor signature of this exception is as follows:

```java
public PolicyTypeNotAvailableForOrganizationException(String message)
```

The only parameter required by this exception is a message that provides more information about the exception. It helps in understanding the reason behind the occurrence of this exception and can be useful while troubleshooting or logging.

## Handling the PolicyTypeNotAvailableForOrganizationException

When encountering the `PolicyTypeNotAvailableForOrganizationException`, it is essential to handle it properly in order to prevent application crashes and ensure a seamless user experience. Here, we will explore two common methods to handle this exception effectively:

### 1. Catching and Logging the Exception

One of the simplest ways to handle the `PolicyTypeNotAvailableForOrganizationException` is to catch the exception and log the details for further analysis or debugging. This approach allows you to identify the cause of the exception and take appropriate actions based on the error message.

Consider the following code snippet that demonstrates how to catch and log the exception:

```java
try {
    // Code snippet to create/update a policy with unsupported policy type
} catch (PolicyTypeNotAvailableForOrganizationException ex) {
    // Log the exception message
    System.out.println("PolicyTypeNotAvailableForOrganizationException: " + ex.getMessage());
    // Take necessary actions based on the exception details
}
```

By logging the exception message using `System.out.println()` or tools like log4j, you can closely monitor and analyze the occurrence of this exception in your application.

### 2. Informing the User or Application Administrators

In some cases, it might be appropriate to inform the end user or application administrators about the occurrence of the `PolicyTypeNotAvailableForOrganizationException`. This can help them understand the reason behind the failure and provide guidance for appropriate actions.

Consider the following code snippet that demonstrates how to inform the user or administrators about the exception:

```java
try {
    // Code snippet to create/update a policy with unsupported policy type
} catch (PolicyTypeNotAvailableForOrganizationException ex) {
    // Display a user-friendly error message
    System.out.println("Sorry, the selected policy type is not available for use within the organization. Please contact your administrator for assistance.");
    // Log the exception message for further analysis
    System.out.println("PolicyTypeNotAvailableForOrganizationException: " + ex.getMessage());
}
```

By providing a user-friendly error message using `System.out.println()` or integrating with a notification system, you ensure that the user is informed about the exception and they can take the necessary steps to resolve the issue.

## Conclusion

In this article, we explored the `PolicyTypeNotAvailableForOrganizationException` of `com.amazonaws.services.organizations.model` in AWS Organizations. By understanding its occurrence, constructor signature, and handling mechanisms, you now have the knowledge to effectively handle this exception and ensure the smooth functioning of your AWS Organizations-based applications.

Remember to catch and log the exception details for better analysis and inform the user or administrators about the occurrence to provide appropriate guidance. By following these practices, you can handle the `PolicyTypeNotAvailableForOrganizationException` effectively and ensure a seamless user experience within your organization.

If you encounter this exception frequently, it might be worth revisiting your policy types and make sure they are supported in AWS Organizations. Check the AWS Organizations API Reference for more information on policy types: [AWS Organizations API Reference](https://docs.aws.amazon.com/organizations/latest/APIReference/Welcome.html)

Stay tuned for more informative articles on AWS Services and effective exception handling!

**References:**
- [AWS Organizations API Reference](https://docs.aws.amazon.com/organizations/latest/APIReference/Welcome.html)
