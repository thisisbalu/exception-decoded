---
title: "InvalidKMSResourceException in AWS Kinesis Firehose"
date: 2023-12-04 09:00:00 -0000
categories: [AWS, AWS Kinesis Firehose]
tags: [aws, kinesisfirehose, com.amazonaws.services.kinesisfirehose.model]
mermaid: true
toc: true
---


*Learn about the InvalidKMSResourceException of com.amazonaws.services.kinesisfirehose.model in AWS Kinesis Firehose and how to handle this exception effectively.*

---

## Introduction

AWS Kinesis Firehose is a powerful service that helps in loading and delivering a large volume of streaming data to other AWS services and external systems. It simplifies the process of ingesting, transforming, and storing data for real-time analytics. However, during the usage of AWS Kinesis Firehose, you may encounter the `InvalidKMSResourceException` when working with encrypted data using AWS Key Management Service (KMS).

In this article, we will delve into the details of the `InvalidKMSResourceException` and understand its causes, impact, and solution. Additionally, we will explore some code examples to demonstrate how to handle and mitigate this exception effectively. So, let's get started!

---

## Understanding InvalidKMSResourceException

The `InvalidKMSResourceException` is an exception that occurs when there is an issue with the AWS Key Management Service (KMS) configuration associated with an AWS Kinesis Firehose delivery stream. This exception specifically indicates that there is a problem with the specified KMS key ARN, which is either incorrect, doesn't exist, or the user doesn't have sufficient permissions to access it.

The exception is thrown by the `com.amazonaws.services.kinesisfirehose.model` package, which provides the necessary classes and methods for interacting with AWS Kinesis Firehose. It is important to handle this exception appropriately to ensure smooth data delivery without any disruptions.

---

## Causes of InvalidKMSResourceException

The `InvalidKMSResourceException` can be caused by several factors, including:

1. **Incorrect KMS key ARN**: If an invalid KMS key ARN is specified while configuring a delivery stream, it will result in the `InvalidKMSResourceException`. It is crucial to ensure that the ARN is accurate and matches a valid KMS key.

2. **Non-existent KMS key**: If the specified KMS key does not exist in the AWS account, the `InvalidKMSResourceException` will be thrown. Verifying the existence of the key is essential to prevent this exception.

3. **Insufficient permissions**: If the user does not have the necessary permissions to access the specified KMS key, the `InvalidKMSResourceException` will occur. Granting the appropriate permissions to the user is vital to avoid this exception.

---

## Mitigating InvalidKMSResourceException

To mitigate the `InvalidKMSResourceException` and ensure smooth operation of AWS Kinesis Firehose delivery streams, follow these steps:

1. **Verify the KMS key ARN**: Double-check the KMS key ARN specified while configuring the delivery stream. Ensure that the ARN is correct and matches a valid KMS key.

```java
CreateDeliveryStreamRequest request = new CreateDeliveryStreamRequest()
    .withDeliveryStreamName("my-delivery-stream")
    .withDeliveryStreamType(DeliveryStreamType.DirectPut)
    .withKmsEncryptionConfig(
        new KinesisFirehoseEncryptionConfig()
            .withAwsKmsKeyId("arn:aws:kms:us-east-1:account-id:key/key-id")
     );
```

2. **Confirm the existence of the KMS key**: Verify that the KMS key specified actually exists in the AWS account. You can check the AWS Management Console or use the AWS CLI to ensure the key is present.

```shell
aws kms describe-key --key-id arn:aws:kms:us-east-1:account-id:key/key-id
```

3. **Check user permissions**: Ensure that the user configuring the delivery stream has the necessary permissions to access the KMS key. Check the IAM policies associated with the user and grant the required permissions if necessary.

---

## Conclusion

The `InvalidKMSResourceException` is an important exception to be aware of while working with AWS Kinesis Firehose and encrypted data. By understanding the causes and following the mitigation steps, you can overcome this exception and ensure the successful delivery of streaming data.

In this article, we explored the details of the `InvalidKMSResourceException` in AWS Kinesis Firehose, discussed its causes and impact, and provided code examples to handle and mitigate the exception effectively. By following the best practices outlined here, you can avoid disruptions in your data processing pipelines.

To learn more about AWS Kinesis Firehose and the associated exception handling, refer to the official AWS documentation:

- [AWS Kinesis Firehose Developer Guide](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html)
- [AWS KMS Key Policy Reference](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html)

Thank you for reading!