---
title: "InvalidKMSResourceException in AWS Kinesis Firehose: A Comprehensive Guide"
date: 2023-12-04 09:00:00 -0000
categories: [AWS, AWS Kinesis Firehose]
tags: [aws, kinesisfirehose, com.amazonaws.services.kinesisfirehose.model]
mermaid: true
toc: true
---


As businesses continue to generate massive amounts of data, real-time streaming and analytics have become indispensable for data-driven decision making. Amazon Kinesis Firehose, a fully managed service offered by AWS, enables developers to reliably load streaming data into data lakes, data stores, and analytics services with ease.

In this article, we will dive deep into the `InvalidKMSResourceException` of the `com.amazonaws.services.kinesisfirehose.model` in AWS Kinesis Firehose. We will explore what this exception means, when and why it occurs, how to handle it, and provide some code examples to illustrate its usage.

## Table of Contents
1. Introduction
2. Understanding the InvalidKMSResourceException
   - What is the InvalidKMSResourceException?
   - When and why does it occur?
   - How to handle the InvalidKMSResourceException
3. Code Examples
4. Conclusion
5. References

## 1. Introduction

Amazon Kinesis Firehose is a powerful service that simplifies the process of loading streaming data into various AWS data storage and analytics platforms. It allows you to capture and transform real-time data from several sources seamlessly.

However, like any other service, Kinesis Firehose can encounter exceptions. One such exception is the `InvalidKMSResourceException`. Let's find out more about it.

## 2. Understanding the InvalidKMSResourceException

### What is the InvalidKMSResourceException?

The `InvalidKMSResourceException` is an exception that indicates that the specified KMS (Key Management Service) resource is not valid. KMS is a managed service by AWS that helps create and control the encryption keys used to encrypt data stored or transmitted by AWS services.

This exception occurs specifically when you configure server-side encryption (SSE) for your Kinesis Firehose delivery stream and provide an invalid or incorrect KMS resource.

### When and why does it occur?

The `InvalidKMSResourceException` can occur in the following situations:

1. **Invalid KMS Key ID**: When creating or updating a delivery stream in Kinesis Firehose, you provide a KMS Key ID as the value for `SSEKMSEncryptionParams.KeyARN` parameter. If the specified KMS Key ID is incorrect or doesn't exist, it will result in the `InvalidKMSResourceException`.

2. **Unassociation of KMS Key**: Suppose you initially configure server-side encryption using KMS for your Kinesis Firehose delivery stream and later delete or remove the associated KMS Key ID from the encrypted delivery stream. In that case, any subsequent operations on the stream will throw an `InvalidKMSResourceException`.

3. **Incorrect KMS Resource Policy**: If the KMS Key associated with your Kinesis Firehose delivery stream has an incorrect or insufficient resource policy that denies the necessary actions for encryption and decryption, it can lead to the `InvalidKMSResourceException`.

### How to handle the InvalidKMSResourceException

To handle the `InvalidKMSResourceException`, you should consider the following steps:

1. **Validate the KMS Key**: Double-check the KMS Key ID you provided when creating or updating your Kinesis Firehose delivery stream. Ensure that the Key ID exists and is correct.

2. **Check KMS Key Association**: If you receive this exception after updating your delivery stream, verify that the correct KMS Key ID is still associated with the stream. If not, reconfigure the stream to use the correct KMS Key ID for server-side encryption.

3. **Review the KMS Resource Policy**: In cases where you encounter the `InvalidKMSResourceException` due to an incorrect KMS resource policy, review and update the policy to allow the necessary actions for encryption and decryption. Refer to the [AWS KMS Resource Policy Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-accounts) for guidance.

## 3. Code Examples

To further understand how to handle the `InvalidKMSResourceException`, let's take a look at some code examples.

### Java Example:

```java
import com.amazonaws.services.kinesisfirehose.AmazonKinesisFirehoseClient;
import com.amazonaws.services.kinesisfirehose.model.CreateDeliveryStreamRequest;
import com.amazonaws.services.kinesisfirehose.model.DeliveryStreamEncryptionConfigurationInput;
import com.amazonaws.services.kinesisfirehose.model.InvalidKMSResourceException;

public class KinesisFirehoseExample {
    public static void main(String[] args) {
        AmazonKinesisFirehoseClient firehoseClient = new AmazonKinesisFirehoseClient();

        String deliveryStreamName = "myDeliveryStream";
        String kmsKeyARN = "arn:aws:kms:us-east-1:123456789012:key/my-kms-key";

        CreateDeliveryStreamRequest createStreamRequest = new CreateDeliveryStreamRequest()
                .withDeliveryStreamName(deliveryStreamName)
                .withSSEKMSEncryptionParams(new DeliveryStreamEncryptionConfigurationInput()
                        .withKeyARN(kmsKeyARN));

        try {
            firehoseClient.createDeliveryStream(createStreamRequest);
            System.out.println("Delivery Stream created successfully.");
        } catch (InvalidKMSResourceException e) {
            System.out.println("Invalid KMS resource: " + e.getMessage());
        }
    }
}
```

In this Java example, we create a new delivery stream in Kinesis Firehose and specify a KMS Key ARN for server-side encryption. If the provided KMS Key ARN is invalid, an `InvalidKMSResourceException` will be thrown.

### Python Example:

```python
import boto3
from botocore.exceptions import InvalidKMSResourceException

client = boto3.client('firehose')

delivery_stream_name = 'myDeliveryStream'
kms_key_arn = 'arn:aws:kms:us-east-1:123456789012:key/my-kms-key'

try:
    response = client.create_delivery_stream(
        DeliveryStreamName=delivery_stream_name,
        DeliveryStreamEncryptionConfigurationInput={
            'KeyARN': kms_key_arn
        }
    )
    print("Delivery Stream created successfully.")
except InvalidKMSResourceException as e:
    print("Invalid KMS resource:", e)
```

This Python example demonstrates the creation of a delivery stream using the AWS SDK for Python (Boto3). If the provided KMS Key ARN is invalid, an `InvalidKMSResourceException` will be raised.

## 4. Conclusion

By understanding the `InvalidKMSResourceException` in AWS Kinesis Firehose, you are better equipped to deal with any related issues during the configuration or operation of your delivery streams. Remember to validate KMS Key IDs, ensure proper KMS Key association, and review the KMS resource policy to handle the exception effectively.

In this article, we explored the causes, handling techniques, and provided code examples in Java and Python to help you troubleshoot and resolve the `InvalidKMSResourceException` in Kinesis Firehose. Following these best practices will ensure a smoother experience while working with Kinesis Firehose.

## 5. References

- AWS Kinesis Firehose Documentation: [https://docs.aws.amazon.com/firehose](https://docs.aws.amazon.com/firehose)
- AWS Key Management Service (KMS) Documentation: [https://docs.aws.amazon.com/kms](https://docs.aws.amazon.com/kms)
- AWS KMS Resource Policy Documentation: [https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-accounts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-accounts)