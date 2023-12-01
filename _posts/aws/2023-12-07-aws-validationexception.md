---
title: "AWS SSM Contacts: Understanding the ValidationException Class"
date: 2023-12-07 09:00:00 -0000
categories: [AWS, AWS SSM Contacts]
tags: [aws, ssmcontacts, com.amazonaws.services.ssmcontacts.model]
mermaid: true
toc: true
---


## Introduction

AWS Systems Manager (SSM) Contacts is a service that helps you integrate your incident management processes with AWS services to create a single place to manage and respond to operational events or issues. It allows you to manage your contacts and escalation plans, and initiate an automated engagement with responders during critical events.

In this article, we will dive deep into the `ValidationException` class provided by the `com.amazonaws.services.ssmcontacts.model` package in AWS SSM Contacts. We will explore its functionalities, use cases, and provide code examples to better understand its usage.

## Understanding the ValidationException Class

The `ValidationException` class is a sub-class of the `AWSException` class provided by the AWS SDK for Java. This exception is thrown when an input value fails to meet the required validation patterns or constraints.

### Use Cases

The `ValidationException` class can be encountered in different scenarios within AWS SSM Contacts. Let's explore some of the use cases where this exception might be thrown:

1. Creating or updating contacts or escalation plans: When creating or updating contacts or escalation plans, the input parameters are validated to ensure they meet the required constraints. If a constraint is violated, a `ValidationException` is thrown.

2. Sending text messages or voice messages: While sending text messages or voice messages using AWS SSM Contacts, the input values are validated for proper format or length. If the validation fails, a `ValidationException` is raised.

3. Managing contacts and escalation plan associations: When associating or disassociating contacts with an escalation plan, the input parameters are checked for validity. If the parameters do not meet the required conditions, a `ValidationException` is thrown.

### Code Examples

Let's explore some code examples to understand how the `ValidationException` class can be used:

#### Example 1: Creating a Contact

```java
import com.amazonaws.services.ssmcontacts.AWSSSMContacts;
import com.amazonaws.services.ssmcontacts.AWSSSMContactsClientBuilder;
import com.amazonaws.services.ssmcontacts.model.CreateContactRequest;
import com.amazonaws.services.ssmcontacts.model.CreateContactResult;
import com.amazonaws.services.ssmcontacts.model.ValidationException;

public class CreateContactExample {
    public static void main(String[] args) {
        AWSSSMContacts ssmContacts = AWSSSMContactsClientBuilder.defaultClient();

        CreateContactRequest request = new CreateContactRequest()
            .withAlias("example-contact")
            .withDisplayName("Example Contact")
            .withPhoneNumber("+1234567890")
            .withPlan("example-escalation-plan");

        try {
            CreateContactResult result = ssmContacts.createContact(request);
            System.out.println("Contact created: " + result.getContactArn());
        } catch (ValidationException e) {
            System.err.println("Invalid input: " + e.getMessage());
        }
    }
}
```

In this example, we create a new contact by calling the `createContact` method of the `AWSSSMContacts` client. If the input parameters fail the validation checks, a `ValidationException` is caught and an error message is displayed.

#### Example 2: Sending a Text Message

```java
import com.amazonaws.services.ssmcontacts.AWSSSMContacts;
import com.amazonaws.services.ssmcontacts.AWSSSMContactsClientBuilder;
import com.amazonaws.services.ssmcontacts.model.SendTextMessageRequest;
import com.amazonaws.services.ssmcontacts.model.SendTextMessageResult;
import com.amazonaws.services.ssmcontacts.model.ValidationException;

public class SendTextMessageExample {
    public static void main(String[] args) {
        AWSSSMContacts ssmContacts = AWSSSMContactsClientBuilder.defaultClient();

        SendTextMessageRequest request = new SendTextMessageRequest()
            .withContactId("example-contact")
            .withContent("This is an example message.");

        try {
            SendTextMessageResult result = ssmContacts.sendTextMessage(request);
            System.out.println("Message sent: " + result.getMessageId());
        } catch (ValidationException e) {
            System.err.println("Invalid message content: " + e.getMessage());
        }
    }
}
```

In this example, we send a text message by calling the `sendTextMessage` method of the `AWSSSMContacts` client. If the message content fails the validation checks, a `ValidationException` is caught and an error message is displayed.

## Conclusion

The `ValidationException` class plays an important role in ensuring data integrity and validating input parameters within AWS SSM Contacts. By using this exception class, developers can handle and respond to validation errors in a structured manner.

Throughout this article, we explored the functionalities and use cases of the `ValidationException` class. We also provided code examples to demonstrate its usage in creating contacts and sending text messages. By leveraging the `ValidationException` class effectively, you can enhance the reliability of your incident management processes.

For further information and in-depth documentation, please refer to the official AWS SSM Contacts API reference [here](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/index.html?com/amazonaws/services/ssmcontacts/model/ValidationException.html).

Happy coding!