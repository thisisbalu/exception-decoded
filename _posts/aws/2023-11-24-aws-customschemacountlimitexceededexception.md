---
title: "CustomSchemaCountLimitExceededException in AWS Simple Systems Management (SSM)"
date: 2023-11-24 09:00:00 -0000
categories: [AWS, AWS Simple Systems Management (SSM)]
tags: [aws, simplesystemsmanagement, com.amazonaws.services.simplesystemsmanagement.model]
mermaid: true
toc: true
---


Are you encountering the `CustomSchemaCountLimitExceededException` while working with AWS Simple Systems Management (SSM)? Don't worry, we've got you covered! In this article, we will explore what this exception means, why it occurs, and how you can handle it effectively in your AWS SSM operations.

## What is CustomSchemaCountLimitExceededException?

The `CustomSchemaCountLimitExceededException` is an exception that can occur while working with the `com.amazonaws.services.simplesystemsmanagement.model` package in AWS Simple Systems Management (SSM). This exception indicates that you have exceeded the maximum limit for custom schemas allowed in SSM.

## Understanding AWS Simple Systems Management (SSM) Custom Schemas

AWS SSM allows you to define custom resource data schemas using the [SSM Agent][1]. These schemas enable you to define the structure and format of your desired configuration data. You can use custom schemas to specify complex data types and improve the integrity of your configuration data.

Custom schemas provide flexibility and allow you to organize your configuration data in a structured manner. However, AWS imposes certain limits on the number of custom schemas you can create to ensure efficient usage and resource allocation.

## Why does CustomSchemaCountLimitExceededException occur?

The `CustomSchemaCountLimitExceededException` occurs when you exceed the maximum count of custom schemas allowed in AWS SSM. By default, the maximum limit is set to 1000 custom schemas per account. If you attempt to create or update a custom schema beyond this limit, the exception will be thrown.

## Handling CustomSchemaCountLimitExceededException

When you encounter the `CustomSchemaCountLimitExceededException`, it is essential to handle it gracefully to ensure smooth execution of your AWS SSM operations. Here are a few steps to help you handle this exception effectively:

1. **Review your existing custom schemas**: Start by reviewing the custom schemas you have already defined in your AWS SSM account. Identify if any obsolete or unused schemas can be removed to free up space for new ones. Remember that each custom schema consumes resources, so removing unnecessary ones can increase your available limit.

2. **Consolidate similar schemas**: Analyze if you have multiple custom schemas that serve similar purposes or contain redundant information. Consider consolidating them into a single schema to optimize resource usage.

3. **Revisit your design choices**: Evaluate your design choices and identify if you can simplify or optimize your custom schemas. Sometimes, complex schemas can be simplified without sacrificing functionality. By revisiting and refining your design, you may be able to reduce the number of custom schemas required.

4. **Request a limit increase**: If your AWS SSM operations genuinely require more than the default limit of 1000 custom schemas, you can request a limit increase. Contact AWS Support and provide a valid use case for the increase, along with any supporting documentation. AWS will assess your request and grant the increase if deemed necessary.

## Code Examples

Let's take a look at some code examples to illustrate how you can handle the `CustomSchemaCountLimitExceededException` using the AWS SDK for Java.

**Java SDK Version 2.x**

```java
import software.amazon.awssdk.services.ssm.model.CustomSchemaCountLimitExceededException;
import software.amazon.awssdk.services.ssm.SsmClient;
import software.amazon.awssdk.services.ssm.model.CreateDocumentRequest;

public class SSMCustomSchemaExample {
    public static void main(String[] args) {
        try {
            SsmClient ssmClient = SsmClient.create();
            CreateDocumentRequest request = CreateDocumentRequest.builder()
                .name("customSchemaExample")
                .content("Some sample custom schema content")
                .build();
            ssmClient.createDocument(request);
        } catch (CustomSchemaCountLimitExceededException e) {
            // Handle the exception - apply the steps outlined earlier in this article
        }
    }
}
```

**Java SDK Version 1.x**

```java
import com.amazonaws.services.simplesystemsmanagement.AWSSimpleSystemsManagement;
import com.amazonaws.services.simplesystemsmanagement.model.CreateDocumentRequest;
import com.amazonaws.services.simplesystemsmanagement.model.CustomSchemaCountLimitExceededException;
import com.amazonaws.services.simplesystemsmanagement.AWSSimpleSystemsManagementClientBuilder;

public class SSMCustomSchemaExample {
    public static void main(String[] args) {
        try {
            AWSSimpleSystemsManagement ssmClient = AWSSimpleSystemsManagementClientBuilder.defaultClient();
            CreateDocumentRequest request = new CreateDocumentRequest()
                .withName("customSchemaExample")
                .withContent("Some sample custom schema content");
            ssmClient.createDocument(request);
        } catch (CustomSchemaCountLimitExceededException e) {
            // Handle the exception - apply the steps outlined earlier in this article
        }
    }
}
```

These code snippets demonstrate a basic example of handling the `CustomSchemaCountLimitExceededException` in AWS SSM. Remember to customize them according to your specific use case and design requirements.

## Conclusion

In this article, we explored the `CustomSchemaCountLimitExceededException` in AWS Simple Systems Management (SSM). We discussed what this exception signifies, why it occurs, and presented various steps to effectively handle it. By following the outlined strategies, you can optimize your custom schemas, manage resource usage efficiently, and ensure smooth execution of your AWS SSM operations.

Remember, AWS provides a comprehensive documentation guide for Custom Schemas, and it's always a good idea to refer to it for detailed reference and up-to-date information.

[1]: https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html