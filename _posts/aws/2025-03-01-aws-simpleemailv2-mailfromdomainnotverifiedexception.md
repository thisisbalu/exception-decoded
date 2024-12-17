---
title: "Understanding MailFromDomainNotVerifiedException in AWS Simple Email Service V2
Example DNS Record (replace <your-domain> accordingly)
Example DNS Records for MAIL FROM"
date: 2025-03-01 09:00:00 -0000
categories: [AWS, AWS Simple Email V2]
tags: [aws, simpleemailv2, com.amazonaws.services.simpleemailv2.model]
mermaid: true
toc: true
---


Amazon Web Services (AWS) Simple Email Service (SES) is an effective way to send and receive emails at scale. However, when configuring custom domains for email sending, you might encounter the `MailFromDomainNotVerifiedException`. Understanding and resolving this issue is vital for a smooth email delivery experience. In this article, we will explore the ins and outs of `MailFromDomainNotVerifiedException`, its causes, and how to resolve it effectively.

## What is MailFromDomainNotVerifiedException?

`MailFromDomainNotVerifiedException` is an exception thrown by AWS SES when an attempt is made to send an email using a custom MAIL FROM domain that has not been verified. Email delivery can be impacted if you send emails from a domain that AWS has not confirmed is under your control. Verifying your MAIL FROM domain enhances your email deliverability by indicating to recipient mail servers that you have legitimate ownership of the domain.

### Common Causes

1. **Unverified Domain**: The MAIL FROM domain you are using is not verified in SES.
2. **Incorrect DNS Settings**: Your DNS settings do not match the verification requirements for AWS SES.
3. **Propagation Issues**: DNS records have been updated, but the changes have not yet propagated.

## How to Handle MailFromDomainNotVerifiedException

To avoid the `MailFromDomainNotVerifiedException`, or to troubleshoot if you encounter it, follow these steps:

### Step 1: Verify Your Domain

1. **Access SES Console**: Log in to the AWS Management Console and navigate to the SES service.
   
2. **Verify Domain**:
   - In the SES dashboard, click on “Domain Settings”.
   - Add your domain by selecting "Verify a New Domain".
   - Follow the instructions provided by AWS, which typically involve adding a TXT record to your DNS configuration.

```bash
Type: TXT
Name: _amazonses.<your-domain>
Value: <verification-token>
```

3. **Check Verification Status**: Once you add the TXT record, the verification status will update in the SES Console. It may take some time due to DNS propagation.

### Step 2: Configure MAIL FROM Domain

1. **Set MAIL FROM Domain**: In the SES Console, navigate to the "Verified Identities" page and choose the verified domain.
2. **Modify MAIL FROM**:
   - Choose "Edit" for the selected verified domain.
   - Enter your desired MAIL FROM domain. For example, if your domain is `example.com`, a common MAIL FROM domain would be `mail.example.com`.

3. **Add Additional DNS Records**: Create the required DNS records for the MAIL FROM domain specified. 

```bash
Type: MX
Name: mail.example.com
Value: 10 feedback-smtp.us-east-1.amazonses.com

Type: TXT
Name: mail.example.com
Value: "v=spf1 include:amazonses.com ~all"
```

### Step 3: Test Email Sending

1. **Send a Test Email**: After ensuring your domain and MAIL FROM domain are correctly configured, you can send a test email.

```java
// Java SDK Example
import software.amazon.awssdk.services.ses.SesClient;
import software.amazon.awssdk.services.ses.model.*;

public class SendEmail {
    public static void main(String[] args) {
        SesClient client = SesClient.create();
        
        SendEmailRequest request = SendEmailRequest.builder()
            .destination(Destination.builder()
                .toAddresses("recipient@example.com")
                .build())
            .message(Message.builder()
                .body(Body.builder()
                    .text(Content.builder().data("Test Email").build())
                    .build())
                .subject(Content.builder().data("Subject").build())
                .build())
            .fromEmailAddress("sender@mail.example.com")
            .build();

        try {
            SendEmailResponse response = client.sendEmail(request);
            System.out.println("Email sent successfully. Message ID: " + response.messageId());
        } catch (MailFromDomainNotVerifiedException e) {
            System.err.println("Error: " + e.getMessage());
        }
    }
}
```

### Step 4: Monitor Email Activity

After sending your emails, monitor the SES dashboard to ensure messages are sent successfully without throwing exceptions. If you continue to experience issues, revisit your domain settings and verification details.

## Conclusion

The `MailFromDomainNotVerifiedException` is a common roadblock when working with AWS SES, but it is easily resolvable through proper domain verification and DNS configuration. Following the guidelines outlined in this article will help you seamlessly send emails from your custom MAIL FROM domain. Ensuring that your MAIL FROM domain is verified not only avoids exceptions but also ensures better deliverability of your emails.

## References

- [AWS SES Documentation](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/Welcome.html)
- [Verifying Email Addresses and Domains in Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-email-addresses.html)
- [Configuring Your MAIL FROM Domain](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/mail-from.html)