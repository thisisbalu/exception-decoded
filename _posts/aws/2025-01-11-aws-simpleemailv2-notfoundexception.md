---
title: "Understanding NotFoundException in AWS Simple Email Service V2"
date: 2025-01-11 09:00:00 -0000
categories: [AWS, AWS Simple Email V2]
tags: [aws, simpleemailv2, com.amazonaws.services.simpleemailv2.model]
mermaid: true
toc: true
---


When working with AWS Simple Email Service (SES) V2, developers may encounter various exceptions, one of the most common being the `NotFoundException`. This article explores what the `NotFoundException` is, the possible causes, and how to handle it in your applications effectively. We will also provide practical code examples and best practices for developers.

## What is NotFoundException?

In Amazon Web Services' Simple Email Service V2, the `NotFoundException` is thrown when a requested resource cannot be located. This could relate to various SES entities, such as domains, email identities, templates, or configuration sets, among others. Understanding this exception helps in debugging issues and ensures that your application handles situations gracefully.

### Common Scenarios for NotFoundException

1. **Missing Email Identity**: If you attempt to send an email using an identity (e.g., an email address or domain) that has not been verified in SES, a `NotFoundException` will occur.
   
2. **Template Not Found**: When trying to retrieve an email template that does not exist.
   
3. **Configuration Set Issues**: If you specify a configuration set that hasn’t been created or was deleted.

## Handling NotFoundException

To ensure your application can handle `NotFoundException` effectively, incorporating try-catch blocks and useful logging will go a long way. Here’s a sample code snippet demonstrating how to handle a `NotFoundException`.

### Example: Sending Email with SES V2

```java
import software.amazon.awssdk.services.sesv2.SesV2Client;
import software.amazon.awssdk.services.sesv2.model.*;

public class EmailSender {
    private final SesV2Client sesClient;

    public EmailSender(SesV2Client client) {
        this.sesClient = client;
    }

    public void sendEmail(String emailAddress) {
        try {
            SendEmailRequest request = SendEmailRequest.builder()
                    .fromEmailAddress("verified@example.com")
                    .destination(Destination.builder()
                            .toAddresses(emailAddress)
                            .build())
                    .content(Content.builder()
                            .simple(SimpleEmailContent.builder()
                                    .subject(Content.builder()
                                            .data("Test Email Subject")
                                            .build())
                                    .body(Body.builder()
                                            .text(Content.builder()
                                                    .data("Hello World")
                                                    .build())
                                            .build())
                                    .build())
                            .build())
                    .build();

            sesClient.sendEmail(request);
            System.out.println("Email sent successfully!");

        } catch (NotFoundException e) {
            System.err.println("Email not found: " + e.getMessage());
            // Handle not found scenarios, e.g., log or alert
        } catch (SesV2Exception e) {
            System.err.println("An error occurred: " + e.awsErrorDetails().errorMessage());
            // General error handling
        }
    }
}
```

In this example, the `sendEmail` method will throw a `NotFoundException` if the `fromEmailAddress` is not verified. Catching the exception allows you to implement specific error handling, such as logging the issue or notifying administrators.

## Logging and Monitoring Best Practices

To effectively handle and monitor `NotFoundException`, consider integrating logging frameworks such as SLF4J or Log4J. This enables you to maintain a log of exceptions which can be incredibly useful for debugging. Below is an example of how to implement logging in the previous example:

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class EmailSender {
    private static final Logger logger = LoggerFactory.getLogger(EmailSender.class);
    private final SesV2Client sesClient;

    public EmailSender(SesV2Client client) {
        this.sesClient = client;
    }

    // sendEmail method remains the same...

    public void sendEmail(String emailAddress) {
        try {
            // ... [same as above]
        } catch (NotFoundException e) {
            logger.error("Email not found: {}", e.getMessage());
            // Handle specific not found scenarios
        } catch (SesV2Exception e) {
            logger.error("An error occurred: {}", e.awsErrorDetails().errorMessage());
            // General error handling
        }
    }
}
```

## Testing for NotFoundException

Before deploying your application, it's vital to conduct thorough testing, especially for scenarios that could generate a `NotFoundException`. Consider writing unit tests to validate the expected exceptions:

```java
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import software.amazon.awssdk.services.sesv2.SesV2Client;
import software.amazon.awssdk.services.sesv2.model.NotFoundException;

import static org.mockito.Mockito.doThrow;

public class EmailSenderTest {

    @Test
    public void testSendEmail_NotFound() {
        SesV2Client mockedClient = Mockito.mock(SesV2Client.class);
        EmailSender emailSender = new EmailSender(mockedClient);

        doThrow(NotFoundException.class).when(mockedClient).sendEmail(Mockito.any());

        emailSender.sendEmail("unverified@example.com");
        // Assertions can be added to verify logging behavior
    }
}
```

This test will mock the SES client and throw a `NotFoundException` when `sendEmail` is called, allowing you to test how your application responds.

## Conclusion

The `NotFoundException` in AWS Simple Email Service V2 serves as a critical indicator of missing resources. Whether you are dealing with email identities, templates, or configurations, understanding how to manage this exception will enhance the robustness of your application. 

By implementing proper exception handling, logging, and testing strategies, developers can ensure that their applications not only handle errors gracefully but also provide a better user experience.

### References

- [AWS SDK for Java v2 Documentation](https://sdk.amazonaws.com/java/api/latest/)
- [AWS Simple Email Service Developer Guide](https://docs.aws.amazon.com/ses/latest/dg/Welcome.html)
- [Amazon SES API Reference - SES V2](https://docs.aws.amazon.com/ses/latest/APIReference/Welcome.html)