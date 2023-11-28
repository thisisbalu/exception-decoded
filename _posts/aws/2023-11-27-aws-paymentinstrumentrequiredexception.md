---
title: "PaymentInstrumentRequiredException in AWS Organizations: A Guide to Managing Payment Instruments "
date: 2023-11-27 09:00:00 -0000
categories: [AWS, AWS Organizations]
tags: [aws, organizations, com.amazonaws.services.organizations.model]
mermaid: true
toc: true
---


Are you struggling with managing payment instruments in your AWS Organizations account? Do you find it challenging to keep track of expenses and payments across multiple accounts and services? Look no further! In this comprehensive guide, we will dive deep into the `PaymentInstrumentRequiredException` model of `com.amazonaws.services.organizations` in AWS Organizations. By the end of this article, you'll be equipped with the knowledge and tools to effectively handle payment instruments, ensuring seamless operations across your organization.

## Table of Contents
1. Introduction to AWS Organizations
2. Understanding `PaymentInstrumentRequiredException`
   - 2.1 What is `PaymentInstrumentRequiredException`?
   - 2.2 When and why does `PaymentInstrumentRequiredException` occur?
3. Handling `PaymentInstrumentRequiredException` properly
   - 3.1 Verifying the payment methods
   - 3.2 Updating payment instruments
       - 3.2.1 Updating the account payment method
       - 3.2.2 Updating the consolidated billing payment method
4. Conclusion
5. References

## Introduction to AWS Organizations
AWS Organizations is a powerful service that allows you to centrally manage multiple AWS accounts within your organization. It simplifies the administration process, provides consolidated billing, and enables fine-grained control over policies and access management.

When working with AWS Organizations, it is essential to understand how payment instruments are managed and the potential challenges that can arise, such as the `PaymentInstrumentRequiredException`.

## Understanding `PaymentInstrumentRequiredException`

### 2.1 What is `PaymentInstrumentRequiredException`?
The `PaymentInstrumentRequiredException` is an exception specific to the `com.amazonaws.services.organizations.model` package in AWS Organizations. It indicates that a payment instrument is required to perform certain actions, such as creating new accounts, enabling policies, or making changes to existing accounts.

### 2.2 When and why does `PaymentInstrumentRequiredException` occur?
The `PaymentInstrumentRequiredException` occurs when a payment instrument, like a credit card or direct debit, is missing or invalid for an AWS account or organization. This exception commonly arises in the following scenarios:

- **Creating new accounts**: When creating a new account within your organization, AWS requires a valid payment instrument to ensure billing and payment accuracy.

- **Making changes to existing accounts**: Certain actions, such as enabling specific services or policies, may trigger the need for a valid payment instrument. This ensures that AWS can charge you appropriately for the services used.

In these cases, the `PaymentInstrumentRequiredException` is thrown to alert you about the missing or invalid payment instrument. It is crucial to handle this exception appropriately to prevent disruptions in your AWS Organizations operations.

## Handling `PaymentInstrumentRequiredException` properly

To overcome the `PaymentInstrumentRequiredException` and ensure smooth operations in your AWS Organizations account, you can follow the steps below:

### 3.1 Verifying the payment methods
Before performing any actions that may require payment instruments, it's essential to ensure that valid payment methods are associated with your AWS account and organization. You can check the payment methods using the AWS Management Console, AWS CLI, or SDKs.

For example, using the AWS CLI, you can check the account-level payment method with the following command:

```console
$ aws organizations describe-account --account-id YOUR_ACCOUNT_ID
```

Similarly, you can check the consolidated billing payment method with the following command:

```console
$ aws organizations describe-organization
```

If the payment methods are missing or invalid, you'll need to update them as explained in the next section.

### 3.2 Updating payment instruments
To resolve the `PaymentInstrumentRequiredException` and ensure uninterrupted operations, you need to update the payment instruments associated with your AWS account and organization. Let's explore two common scenarios in which you may need to update the payment instruments:

#### 3.2.1 Updating the account payment method
If the payment method for a specific account is missing or invalid, you can update it using the `aws organizations update-account` command. This command allows you to specify the account ID and the new payment method.

Here's an example of updating the payment method for an account using the AWS CLI:

```console
$ aws organizations update-account \
    --account-id YOUR_ACCOUNT_ID \
    --payment-instrument-type CREDIT_CARD \
    --credit-card-info '{"creditCardNumber": "**** **** **** ****", "expirationMonth": "12", "expirationYear": "2025", "cvc": "***"}'
```

Make sure to replace `YOUR_ACCOUNT_ID` and the credit card info with the respective values.

#### 3.2.2 Updating the consolidated billing payment method
If the consolidated billing payment method for your organization is missing or invalid, you can update it using the AWS Management Console or the AWS CLI.

To update the consolidated billing payment method via the AWS CLI, use the following command:

```console
$ aws organizations update-organization \
    --awspayment-instrument-type BILLING_CARD \
    --billing-card-info '{"creditCardNumber": "**** **** **** ****", "expirationMonth": "12", "expirationYear": "2025", "cvc": "***"}'
```

Remember to replace the billing card info with your payment instrument details.

By following these steps, you can ensure that you have valid payment instruments associated with your AWS accounts and organization, thereby avoiding the `PaymentInstrumentRequiredException`.

## Conclusion
The `PaymentInstrumentRequiredException` is an essential aspect of managing payment instruments in AWS Organizations. By understanding the nature of the exception and following the recommended steps, you can ensure a seamless experience while managing your accounts and services within AWS Organizations. Always verify and update your payment instruments when necessary to prevent any interruptions or issues related to payments.

Now that you have a good grasp of the `PaymentInstrumentRequiredException` in AWS Organizations, you are equipped to take your payment instrument management to the next level.

## References
- [AWS Organizations Developer Guide](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html)
- [AWS CLI Command Reference - Organizations](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/organizations/index.html)
- [AWS SDKs](https://aws.amazon.com/tools/)

