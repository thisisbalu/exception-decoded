---
title: "CloudHsmClusterInUseException of com.amazonaws.services.kms.model in AWS KMS: Explained"
date: 2024-05-31 09:00:00 -0000
categories: [AWS, AWS KMS]
tags: [aws, kms, com.amazonaws.services.kms.model]
mermaid: true
toc: true
---


## Introduction
Amazon Web Services (AWS) Key Management Service (KMS) is a fully managed service that allows you to create and control encryption keys, as well as encrypt and decrypt data using those keys. It helps you protect sensitive information and meet compliance requirements.

In this article, we will discuss one specific exception thrown by the AWS KMS API: `CloudHsmClusterInUseException`. We will dive into what this exception means, why it may occur, and how to handle it effectively. So, let's get started!

## What is `CloudHsmClusterInUseException`?
`CloudHsmClusterInUseException` is an exception class provided by the `com.amazonaws.services.kms.model` package in the AWS KMS Java SDK. It is thrown when you attempt to delete a custom key store in KMS that is still associated with a CloudHSM cluster.

A CloudHSM cluster is a group of hardware security modules (HSMs) used to generate and store cryptographic keys securely. When a custom key store is associated with a CloudHSM cluster, it means that it is using the HSMs for key operations.

## Why does the exception occur?
This exception occurs when you try to delete a custom key store using the `DeleteCustomKeyStore` API and the custom key store is still associated with a CloudHSM cluster. AWS KMS does not allow you to delete a key store that is actively using a CloudHSM cluster. This restriction ensures that you don't accidentally delete a custom key store that is being used by another application or service.

## How to handle `CloudHsmClusterInUseException`
When you encounter a `CloudHsmClusterInUseException`, there are a few steps you can take to handle it effectively:

1. **Check if the CloudHSM cluster is being used**: The first step is to verify whether the CloudHSM cluster associated with the custom key store is still actively being used. You can do this by checking if any other application or service relies on the key store's CloudHSM cluster. If it is actively in use, you should investigate further before proceeding with the deletion.

2. **Disable the key store**: If you determine that the custom key store is no longer needed and the associated CloudHSM cluster is not actively being used, you can disable the key store using the `DisableCustomKeyStore` API. This ensures that no new key operations can be performed with the key store.

3. **Wait for CloudHSM cluster utilization to drop**: After disabling the key store, you might need to wait for a while until the CloudHSM cluster's utilization drops to zero. This process may take some time, depending on the workload and key usage.

4. **Delete the custom key store**: Once the CloudHSM cluster's utilization drops to zero and you're confident that the custom key store is no longer actively used, you can attempt to delete the key store again using the `DeleteCustomKeyStore` API.

Handling this exception effectively ensures that you don't inadvertently remove a key store being utilized by other critical applications or services.

## Code Examples
Let's take a look at some code examples demonstrating how to handle the `CloudHsmClusterInUseException` using the AWS KMS Java SDK:

```java
import com.amazonaws.services.kms.AWSKMS;
import com.amazonaws.services.kms.AWSKMSClientBuilder;
import com.amazonaws.services.kms.model.DisableCustomKeyStoreRequest;
import com.amazonaws.services.kms.model.DeleteCustomKeyStoreRequest;
import com.amazonaws.services.kms.model.CloudHsmClusterInUseException;

public class CustomKeyStoreDeletionExample {
    public static void main(String[] args) {
        // Set up AWS KMS client
        AWSKMS kmsClient = AWSKMSClientBuilder.defaultClient();

        // Define custom key store ID
        String customKeyStoreId = "myCustomKeyStoreId";

        // Disable the custom key store
        DisableCustomKeyStoreRequest disableRequest =
                new DisableCustomKeyStoreRequest().withCustomKeyStoreId(customKeyStoreId);
        kmsClient.disableCustomKeyStore(disableRequest);

        // Wait for CloudHSM cluster utilization to drop
        // ... code to wait for utilization to reach zero ...

        // Attempt to delete the custom key store
        DeleteCustomKeyStoreRequest deleteRequest =
                new DeleteCustomKeyStoreRequest().withCustomKeyStoreId(customKeyStoreId);
        try {
            kmsClient.deleteCustomKeyStore(deleteRequest);
            System.out.println("Custom key store deleted successfully.");
        } catch (CloudHsmClusterInUseException ex) {
            System.err.println("CloudHsmClusterInUseException: " + ex.getMessage());
            // Handle the exception gracefully
        }
    }
}
```

In this code example, we disable the custom key store using the `DisableCustomKeyStoreRequest` API, then wait for the utilization to drop to zero before attempting to delete the store using the `DeleteCustomKeyStoreRequest` API. If the `CloudHsmClusterInUseException` occurs during deletion, we catch it and handle it accordingly.

## Conclusion
In this article, we explored the `CloudHsmClusterInUseException` of the `com.amazonaws.services.kms.model` package in AWS KMS. We learned what this exception means, why it may occur, and how to handle it effectively. By performing the necessary checks and following the guidelines provided, you can avoid accidental deletion of custom key stores that are still actively used. 

To learn more about AWS KMS, visit the official [AWS KMS documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html).

*Note: The code examples in this article assume that you have already set up your AWS KMS client and have the necessary permissions to perform the mentioned operations.*

*References:*
- AWS KMS Official Documentation: [https://docs.aws.amazon.com/kms/latest/developerguide/overview.html](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)