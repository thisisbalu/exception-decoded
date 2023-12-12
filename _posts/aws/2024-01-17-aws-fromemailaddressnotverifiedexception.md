---
title: "Catchy title: Overcoming the FromEmailAddressNotVerifiedException in AWS Simple Email Service"
date: 2024-01-17 09:00:00 -0000
categories: [AWS, AWS Simple Email Service]
tags: [aws, simpleemail, com.amazonaws.services.simpleemail.model]
mermaid: true
toc: true
---


**Abstract:** 
In this article, we will delve into the details of the `FromEmailAddressNotVerifiedException` error in the AWS Simple Email Service (SES). We will discuss what causes this exception, how to handle it, and provide code examples to demonstrate its resolution. By the end, you'll have a comprehensive understanding of this issue and be equipped with the knowledge to overcome it effectively.

## Introduction

The FromEmailAddressNotVerifiedException is a common error encountered while working with the AWS Simple Email Service. This exception is raised when attempting to send an email using SES, but the From email address has not been verified in the AWS console. In this article, we will walk you through the process of handling this exception, providing code snippets and showcasing the steps to resolve it.

## Understanding the FromEmailAddressNotVerifiedException

### What causes the FromEmailAddressNotVerifiedException?

When sending emails through the AWS Simple Email Service, it requires the From email address to be verified in order to prevent abuse and ensure the stability of the service. If the From email address has not been verified in the AWS SES console, the service restricts sending email from unverified addresses and throws the `FromEmailAddressNotVerifiedException` during the SendMessage API call.

### How to handle the exception?

To overcome the `FromEmailAddressNotVerifiedException`, the From email address must be verified in the AWS SES console. This ensures that all email communication originates from known and trusted sources. By verifying email addresses, you prevent unauthorized abuse of the service and maintain a reputable standing.

To handle the exception and resolve this issue, follow these steps:

1. Log in to your AWS Management Console and navigate to the [AWS Simple Email Service](https://console.aws.amazon.com/ses/) page.
2. Open the "Email Addresses" tab and click on "Verify a New Email Address".
3. Enter the email address you want to use for the From field and click on the "Verify This Email Address" button.
4. AWS SES will then send a verification email to the specified address. Open the email and click on the verification link.
5. Finally, you will receive a confirmation message indicating successful verification.

Once the From email address is successfully verified, you can proceed to send emails without encountering the `FromEmailAddressNotVerifiedException`.

### Code Examples

Let's look at some code examples to demonstrate how to handle and resolve the `FromEmailAddressNotVerifiedException` programmatically using the AWS SDK for Java.

**1. Sending an email with the Java SDK:**

```java
import com.amazonaws.services.simpleemail.AmazonSimpleEmailService;
import com.amazonaws.services.simpleemail.AmazonSimpleEmailServiceClientBuilder;
import com.amazonaws.services.simpleemail.model.*;

public class EmailSender {
    public static void main(String[] args) {
        // Configure the AWS credentials and region
        String accessKey = "YOUR_AWS_ACCESS_KEY";
        String secretKey = "YOUR_AWS_SECRET_KEY";
        String region = "us-west-2";

        // Create the AWS SES client
        AmazonSimpleEmailService client = AmazonSimpleEmailServiceClientBuilder.standard()
                                                .withRegion(region)
                                                .withCredentials(new AWSStaticCredentialsProvider(
                                                        new BasicAWSCredentials(accessKey, secretKey)))
                                                .build();

        // Create the email request
        SendEmailRequest request = new SendEmailRequest()
                                        .withSource("your_verified_email@example.com")
                                        .withDestination(new Destination()
                                                            .withToAddresses("recipient@example.com"))
                                        .withMessage(new Message()
                                                        .withSubject(new Content()
                                                                       .withCharset("UTF-8").withData("Hello, world!"))
                                                        .withBody(new Body()
                                                                    .withText(new Content()
                                                                                  .withCharset("UTF-8").withData("This is the message body."))));

        // Attempt to send the email
        try {
            SendEmailResult result = client.sendEmail(request);
            System.out.println("Email sent successfully!");
        } catch (FromEmailAddressNotVerifiedException e) {
            System.out.println("Error: From email address is not verified.");
            System.out.println("Please verify the email address in the AWS SES console.");
        }
    }
}
```

**2. Handling the exception with error handling and verification code:**

```java
try {
    SendEmailResult result = client.sendEmail(request);
    System.out.println("Email sent successfully!");
} catch (AmazonServiceException e) {
    if (e.getErrorCode().equals("MessageRejected")) {
        if (e.getMessage().contains("FromEmailAddressNotVerifiedException")) {
            System.out.println("Error: From email address is not verified.");
            System.out.println("Please verify the email address in the AWS SES console.");
        } else {
            System.out.println("Error: " + e.getMessage());
        }
    } else {
        System.out.println("Error: " + e.getMessage());
    }
}
```

With these code examples, you can handle and detect the `FromEmailAddressNotVerifiedException` gracefully without causing any application disruptions.

## Conclusion

By verifying your From email addresses in the AWS SES console, you can prevent the `FromEmailAddressNotVerifiedException` and ensure the smooth delivery of your emails. We have explored the causes, handling steps, and provided code examples to guide you through the resolution process. Remember, maintaining the integrity and trustworthiness of your email service is crucial for successful communication with your recipients.

In this article, we have equipped you with the necessary knowledge to overcome the `FromEmailAddressNotVerifiedException` gracefully. Now, you can confidently send emails through the AWS Simple Email Service without concerns about this exception hindering your communication.

For further details, refer to the official [AWS SES documentation](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/).

Happy email sending with AWS SES!