---
title: "**PolicyTypeNotAvailableForOrganizationException in AWS Organizations**"
date: 2023-12-27 09:00:00 -0000
categories: [AWS, AWS Organizations]
tags: [aws, organizations, com.amazonaws.services.organizations.model]
mermaid: true
toc: true
---


Organizations have become an essential part of managing and governing multiple AWS accounts. With the advent of AWS Organizations, account management has become a seamless process. However, as with any service, there can be exceptions that can hinder the smooth functioning of an application. One such exception is the `PolicyTypeNotAvailableForOrganizationException`.

In this article, we will explore the `PolicyTypeNotAvailableForOrganizationException` of the `com.amazonaws.services.organizations.model` in AWS Organizations. We will understand its implications, causes, and possible solutions. So, let's dive in!

## Understanding the PolicyTypeNotAvailableForOrganizationException

The `PolicyTypeNotAvailableForOrganizationException` is an exception that occurs when a specific policy type is not available for an organization. AWS Organizations provides various policy types, such as service control policies (SCPs) and tag policies, to enforce fine-grained permissions and organizational boundaries. However, in some cases, certain policy types may not be available for certain organizations.

This exception is thrown when a requested policy type is not supported by the organization. For example, if an organization attempts to create an SCP of a certain type, but that type is not supported by the organization, the `PolicyTypeNotAvailableForOrganizationException` will be thrown.

## Understanding the Causes

There can be several causes for the `PolicyTypeNotAvailableForOrganizationException`. Let's discuss some possible scenarios:

1. **Unsupported Policy Type**: The organization may be trying to use a policy type that is not supported by AWS Organizations. It is essential to refer to the AWS Organizations documentation to ensure the policy type being used is supported.

2. **Account Limitations**: Some policy types have specific requirements or limitations on the number of accounts that can use them. It is crucial to check these limitations and ensure that they are not exceeded.

3. **Account Setup**: The account trying to perform an action involving policy types may not be fully set up or consolidated under the organization. This can lead to inconsistencies and result in the `PolicyTypeNotAvailableForOrganizationException`.

## Resolving the PolicyTypeNotAvailableForOrganizationException

When encountering the `PolicyTypeNotAvailableForOrganizationException`, a few steps can be taken to resolve the issue. Let's explore some possible solutions:

1. **Check AWS Organizations Documentation**: The first step is to refer to the AWS Organizations documentation and verify if the policy type being used is indeed supported by the organization. Each policy type has specific prerequisites and requirements. By ensuring compliance with these prerequisites, you can avoid the `PolicyTypeNotAvailableForOrganizationException`.

Example:

```java
// Check if 'SERVICE_CONTROL_POLICY' type is supported by the organization
DescribeOrganizationResponse organizationResponse = organizationsClient.describeOrganization();

boolean isSCPTypeSupported = organizationResponse.isEnableAllFeaturesSupported(ServiceControlPolicyType.SERVICE_CONTROL_POLICY);

if (!isSCPTypeSupported) {
    throw new PolicyTypeNotAvailableForOrganizationException("SERVICE_CONTROL_POLICY");
}
```

2. **Review Account Configurations**: Verify that the accounts involved in the action have been properly set up and consolidated under the organization. This includes checking if any necessary account linking or organizational unit assignments have been completed.

Example:

```java
// Check if account is in the organization
ListAccountsResponse listAccountsResponse = organizationsClient.listAccounts();

boolean isAccountInOrganization = listAccountsResponse.getAccounts().stream()
        .map(Account::getId)
        .anyMatch(accountId -> accountId.equals(targetAccountId));

if (!isAccountInOrganization) {
    throw new PolicyTypeNotAvailableForOrganizationException("Account not found in the organization");
}
```

3. **Validate Policy Type Limitations**: If a policy type has limitations on the number of accounts it can be applied to, ensure that those limitations are not exceeded. You can retrieve the account count and compare it against the policy type limitations.

Example:

```java
// Check if the account limit is within the allowed range
ListAccountsResponse listAccountsResponse = organizationsClient.listAccounts();

int accountCount = listAccountsResponse.getAccounts().size();

if (accountCount > MAX_ALLOWED_ACCOUNTS) {
    throw new PolicyTypeNotAvailableForOrganizationException("Account limit exceeded");
}
```

By following these steps and incorporating proper error handling, you can effectively handle and resolve the `PolicyTypeNotAvailableForOrganizationException` in your applications.

## Conclusion

In this article, we explored the `PolicyTypeNotAvailableForOrganizationException` of the `com.amazonaws.services.organizations.model` in AWS Organizations. We understood the causes, implications, and possible solutions to resolve this exception. By referring to the AWS Organizations documentation, reviewing account configurations, and validating policy type limitations, you can effectively handle this exception and ensure the smooth functioning of your applications.

Keep in mind that AWS Organizations evolve over time, and new policy types may be introduced or existing ones may be modified. It is essential to stay up to date with the AWS Organizations documentation and any related AWS announcements to ensure the ongoing integrity and security of your systems.

Happy coding!

**References:**
- [AWS Organizations Documentation](https://docs.aws.amazon.com/organizations/index.html)
- [AWS SDK for Java - AWS Organizations](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/examples-organizations.html)

*Estimated reading time: 15 minutes*