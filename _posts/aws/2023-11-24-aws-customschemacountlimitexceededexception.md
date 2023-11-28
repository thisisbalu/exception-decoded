---
title: "CustomSchemaCountLimitExceededException in AWS Simple Systems Management (SSM)"
date: 2023-11-24 09:00:00 -0000
categories: [AWS, AWS Simple Systems Management (SSM)]
tags: [aws, simplesystemsmanagement, com.amazonaws.services.simplesystemsmanagement.model]
mermaid: true
toc: true
---


Have you encountered the **CustomSchemaCountLimitExceededException** while working with AWS Simple Systems Management (SSM)? If so, don't worry, you're not alone. In this article, we will dive deep into this exception, understand its meaning, and explore possible solutions to mitigate the issue. So, let's get started!

## Understanding CustomSchemaCountLimitExceededException

The **CustomSchemaCountLimitExceededException** is an exception class in the `com.amazonaws.services.simplesystemsmanagement.model` package of AWS Simple Systems Management (SSM). This exception is thrown when you exceed the limit of the number of custom schemas defined for the parameter specified in the AWS SSM service.

To provide some context, AWS SSM enables you to manage your infrastructure at scale by automating the configuration and ongoing maintenance of your EC2 instances, on-premises servers, and virtual machines. It helps you maintain compliance and keep your infrastructure secure. The service offers various capabilities such as configuring instances, gathering inventory, patching, and executing commands at scale.

## Causes of the Exception

The **CustomSchemaCountLimitExceededException** occurs when you attempt to define more custom schemas for a parameter than the allowed limit. By default, AWS SSM allows a maximum of 10 custom schemas per parameter. A custom schema allows you to define additional structured data for your parameter. This extra data can be useful to provide additional context or metadata for your configuration.

## Handling the Exception

When you encounter the **CustomSchemaCountLimitExceededException**, there are a few steps you can take to handle it effectively. Let's explore some possible solutions below.

### 1. Review and Optimize the Number of Custom Schemas

The first step is to review the number of custom schemas defined for the parameter that caused the exception. Evaluate if you really need all the custom schemas or if some of them can be consolidated or removed. By optimizing the number of custom schemas, you can stay within the allowed limit and prevent the exception from occurring.

Here's an example of how to retrieve the custom schema count for a parameter using the AWS SDK for Java:

```java
import com.amazonaws.services.simplesystemsmanagement.AWSSimpleSystemsManagement;
import com.amazonaws.services.simplesystemsmanagement.model.DescribeDocumentRequest;
import com.amazonaws.services.simplesystemsmanagement.model.DescribeDocumentResult;

AWSSimpleSystemsManagement ssmClient = AWSSimpleSystemsManagementClient.builder().build();

DescribeDocumentRequest describeDocumentRequest = new DescribeDocumentRequest()
    .withName("your-document-name");

DescribeDocumentResult describeDocumentResult = ssmClient.describeDocument(describeDocumentRequest);

int customSchemaCount = describeDocumentResult.getCustomSchemaCount();
System.out.println("Custom schema count: " + customSchemaCount);
```

By using the `DescribeDocumentRequest` API, you can retrieve the number of custom schemas defined for a specific document. Once you have this count, you can assess if any optimization is required.

### 2. Consider Refactoring Document Structure

If you find that you genuinely need more custom schemas than the allowed limit, it might be worth reconsidering the structure of your documents. You can explore the possibility of consolidating the information into fewer parameters or documents. Refactoring the document structure can help you stay within the limit and ensure smooth execution without encountering the **CustomSchemaCountLimitExceededException**.

### 3. Request Custom Limit Increase

If optimizing or refactoring doesn't resolve the issue, you can consider requesting a limit increase from AWS Support. Reach out to support and provide the necessary details about your use case and why you require a higher number of custom schemas. AWS Support will review your request and assess if they can increase the limit for your account.

### 4. Catch and Handle the Exception

While working with the AWS SDKs, it's essential to handle exceptions gracefully. By catching the **CustomSchemaCountLimitExceededException**, you can provide informative error messages to users and take appropriate actions to resolve the issue.

Here's an example of how to catch the exception in Java:

```java
try {
    // AWS SSM API call that throws CustomSchemaCountLimitExceededException
} catch (com.amazonaws.services.simplesystemsmanagement.model.CustomSchemaCountLimitExceededException ex) {
    // Handle the exception here
    System.out.println("Custom schema count limit exceeded for the parameter.");
}
```

By catching the exception, you can notify users about the exceeded limit and prompt them to optimize their custom schema count or follow other resolution steps.

## Conclusion

In this article, we explored the **CustomSchemaCountLimitExceededException** in AWS Simple Systems Management (SSM). We learned about its causes and discussed various steps to handle the exception effectively. Whether it's optimizing the number of custom schemas, refactoring document structure, requesting a limit increase, or catching the exception, you now have several strategies to overcome this limitation.

To avoid encountering the **CustomSchemaCountLimitExceededException** in the first place, it's crucial to plan and design your AWS SSM configurations carefully. By understanding the limits and optimizing your schemas, you can ensure a smoother experience while managing your infrastructure.

Stay tuned to the [official AWS SSM documentation](https://docs.aws.amazon.com/systems-manager/latest/APIReference/Welcome.html) to learn more about the service and its capabilities. Remember, always keep an eye on the number of custom schemas and plan accordingly to avoid surprises in the future.

Happy management with AWS Simple Systems Management (SSM)!

*Estimated reading time: 15 minutes*