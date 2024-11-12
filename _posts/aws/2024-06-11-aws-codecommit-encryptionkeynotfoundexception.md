---
title: "**Getting Started with EncryptionKeyNotFoundException in AWS CodeCommit**"
date: 2024-06-11 09:00:00 -0000
categories: [AWS, AWS CodeCommit]
tags: [aws, codecommit, com.amazonaws.services.codecommit.model]
mermaid: true
toc: true
---


---

## Introduction

With the increased use of cloud-based solutions and the need for secure data storage, encryption has become an essential component of modern software systems. Amazon Web Services (AWS) offers CodeCommit, a fully-managed source control service that helps teams collaborate on code securely. In this article, we will dive into the `EncryptionKeyNotFoundException` of `com.amazonaws.services.codecommit.model` in AWS CodeCommit and explore how to handle this exception effectively.

## Understanding EncryptionKeyNotFoundException

The `EncryptionKeyNotFoundException` is a specific exception class in the AWS CodeCommit SDK, which is thrown when the specified encryption key is not found. This exception occurs when attempting to access a repository that is encrypted with a customer-managed AWS Key Management Service (KMS) key.

## Handling EncryptionKeyNotFoundException

When encountering the `EncryptionKeyNotFoundException`, there are several steps you can take to handle the exception effectively:

### 1. Review the Encryption Key Configuration

First, ensure that the encryption key is correctly configured for the repository. Double-check the Key ARN (Amazon Resource Name) used for encryption during repository creation or update. Pay attention to the region and alias of the key, as well as its permissions.

### 2. Verify Encryption Key Access Permissions

Verify that the AWS Identity and Access Management (IAM) user or role executing the operation has the necessary permissions to access the encryption key. Ensure that the IAM policy associated with the user/role allows the `kms:Decrypt` action on the specified key.

Here's an example of how you can grant the necessary permissions to an IAM user:

```java
import com.amazonaws.services.kms.AWSKMS;
import com.amazonaws.services.kms.AWSKMSClientBuilder;
import com.amazonaws.services.kms.model.KeyMetadata;
import com.amazonaws.services.identitymanagement.AmazonIdentityManagement;
import com.amazonaws.services.identitymanagement.AmazonIdentityManagementClientBuilder;

String keyId = "arn:aws:kms:us-west-2:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab";
String iamUserName = "codecommit-user";
String policyDocument = "{ " +
    "  \"Version\": \"2012-10-17\"," +
    "  \"Statement\": [" +
    "      {" +
    "          \"Sid\": \"AllowKMSDecrypt\"," +
    "          \"Effect\": \"Allow\"," +
    "          \"Action\": [\"kms:Decrypt\"]," +
    "          \"Resource\": [\"" + keyId + "\"]" +
    "      }" +
    "  ]" +
    "}";

// Associate the policy with the IAM user
AmazonIdentityManagement iam = AmazonIdentityManagementClientBuilder.defaultClient();
iam.putUserPolicy(iamUserName, "CodeCommitKeyPermission", policyDocument);

// Check if the user has access to decrypt the encryption key
AWSKMS kms = AWSKMSClientBuilder.defaultClient();
KeyMetadata keyMetadata = kms.describeKey(keyId).getKeyMetadata();
if (keyMetadata.getEncryptionAlgorithms().contains("SYMMETRIC_DEFAULT")) {
    System.out.println("User has access to decrypt the encryption key.");
} else {
    System.out.println("User does not have access to decrypt the encryption key.");
}
```

### 3. Check the Encryption Key Availability

Ensure that the encryption key specified is active and available in the AWS Key Management Service (KMS). If the key is pending deletion or disabled, it cannot be used for decryption, resulting in the `EncryptionKeyNotFoundException`.

To verify the status of the encryption key, you can use the `describeKey` method from the AWS Java SDK's `AWSKMS` class. Here's an example:

```java
AWSKMS kms = AWSKMSClientBuilder.defaultClient();
String keyId = "arn:aws:kms:us-west-2:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab";
KeyMetadata keyMetadata = kms.describeKey(keyId).getKeyMetadata();

if (keyMetadata.getKeyState().equals("Enabled")) {
    System.out.println("The encryption key is active and available.");
} else {
    System.out.println("The encryption key is not active or available.");
}
```

### 4. Catch and Handle the Exception

In your code, make sure to implement exception handling to catch the `EncryptionKeyNotFoundException` specifically. By catching and handling the exception, you can gracefully handle the scenario when the encryption key is not found.

```java
import com.amazonaws.services.codecommit.AWSCodeCommit;
import com.amazonaws.services.codecommit.AWSCodeCommitClientBuilder;
import com.amazonaws.services.codecommit.model.EncryptionKeyNotFoundException;

String repositoryName = "my-repository";

try {
    AWSCodeCommit codeCommitClient = AWSCodeCommitClientBuilder.defaultClient();
    // Your CodeCommit operations here
} catch (EncryptionKeyNotFoundException e) {
    // Handle the encryption key not found scenario
    System.out.println("The encryption key for repository " + repositoryName + " was not found.");
}
```

### 5. Troubleshooting EncryptionKeyNotFoundException

If the above steps do not resolve the `EncryptionKeyNotFoundException`, consider the following troubleshooting tips:

- Verify that the encryption key ARN is correct, including the region and alias.
- Ensure that the encryption key is available and enabled in the AWS Key Management Service (KMS).
- Check the IAM user/role permissions associated with the encryption key.

## Conclusion

In this article, we explored the `EncryptionKeyNotFoundException` of `com.amazonaws.services.codecommit.model` in AWS CodeCommit and learned how to effectively handle this exception. By reviewing the encryption key configuration, verifying permissions, and checking the availability of the encryption key, you can resolve the exception and ensure the secure use of AWS CodeCommit.

Remember to stay vigilant when handling exceptions related to encryption keys and regularly review the security aspects of your AWS CodeCommit repositories to maintain the highest level of data protection.

To learn more about AWS CodeCommit and its features, refer to the official [AWS CodeCommit documentation](https://docs.aws.amazon.com/codecommit/latest/userguide/Welcome.html).

---

*Duration: 15 minutes*