---
title: "Title: Exploring NotFoundException in AWS Pricing: Solving Common Issues with com.amazonaws.services.pricing.model
        Pricing API call
        Process the pricing details"
date: 2024-05-27 09:00:00 -0000
categories: [AWS, AWS Pricing]
tags: [aws, pricing, com.amazonaws.services.pricing.model]
mermaid: true
toc: true
---


## Introduction
When working with AWS Pricing, you may encounter situations where you want to retrieve pricing information programmatically using the AWS SDK. However, it's not uncommon to face the `NotFoundException` when trying to retrieve pricing details for a specific service or product. In this article, we will dive into the details of this exception, understand its common causes, and explore various strategies to solve the issue effectively.

## Table of Contents

1. Understanding `NotFoundException`
2. Common Causes of `NotFoundException`
   - Missing product or service
   - Invalid region or availability zone
   - Permission restrictions
3. Handling `NotFoundException` gracefully
   - Error handling with try-catch blocks
   - Implementing retries with exponential backoff
4. Troubleshooting `NotFoundException`
5. Conclusion
6. References

## 1. Understanding `NotFoundException`
The `NotFoundException` belongs to the `com.amazonaws.services.pricing.model` package in the AWS SDK. It is an exception that occurs when the requested pricing information is not found in the AWS Pricing service. This exception is often returned as a response when attempting to retrieve pricing details for a specific resource, such as an EC2 instance or an S3 bucket.

## 2. Common Causes of `NotFoundException`

### Missing product or service
One of the common causes of the `NotFoundException` is the absence of the requested product or service in the AWS Pricing service. It's essential to ensure that you are referring to a valid and available product or service within the desired AWS region. Due to constant updates and changes in AWS pricing, some products or services may not be immediately available or might have been deprecated.

### Invalid region or availability zone
Another potential cause of the `NotFoundException` is specifying an invalid AWS region or availability zone. Ensure that you provide the correct region and availability zone information when querying pricing details. Refer to the AWS documentation or the AWS CLI's available regions and availability zones to validate your inputs.

### Permission restrictions
The AWS Pricing API may require specific permissions to retrieve pricing details. If your application lacks the necessary permissions, it can result in the `NotFoundException`. Make sure your AWS credentials have appropriate permissions to access the pricing information for your desired resource. Check your IAM policies and ensure they include the required permissions for the Pricing API calls.

## 3. Handling `NotFoundException` gracefully

### Error handling with try-catch blocks
When making AWS Pricing API calls, it's crucial to handle exceptions gracefully, including the `NotFoundException`. By encapsulating your code within try-catch blocks, you can catch the `NotFoundException` and handle it appropriately. Here's a Java code example:

```java
try {
    // Pricing API call
    PricingClient client = new PricingClient();
    PricingDetails pricingDetails = client.getPricingDetails(productId);
    // Process the pricing details
} catch (NotFoundException e) {
    System.out.println("Pricing details not found: " + e.getMessage());
    // Handle the exception gracefully
}
```

### Implementing retries with exponential backoff
In some cases, the `NotFoundException` may arise due to temporary issues or network glitches. To mitigate such scenarios, you can implement retries with exponential backoff. By retrying the request after a certain delay, you increase the chances of successfully retrieving the pricing details. Here's a Python code example using the AWS SDK for Python (Boto3):

```python
import botocore
import time

retry_count = 3
retry_delay = 1

while retry_count > 0:
    try:
        response = pricing_client.get_pricing_details(ProductId=product_id)
        break  # Exit the loop if successful
    except botocore.exceptions.NotFoundException as e:
        print(f"Pricing details not found: {e}")
        break  # Handle exception gracefully

    retry_count -= 1
    time.sleep(retry_delay)
    retry_delay *= 2  # Exponential backoff
```

## 4. Troubleshooting `NotFoundException`
If you continue to face the `NotFoundException` even after ensuring the correctness of the product, region, and permissions, further troubleshooting might be necessary. Consider the following actions:

- Check AWS service health status to ensure the Pricing service is functioning correctly.
- Contact AWS support if the issue persists or if you suspect an underlying problem with the Pricing service.

## 5. Conclusion
The `NotFoundException` in the `com.amazonaws.services.pricing.model` package can be encountered while retrieving pricing information from AWS Pricing. This exception can arise due to missing products, invalid region or availability zone, or permission restrictions. By understanding the potential causes and implementing appropriate strategies, such as error handling and retries, you can efficiently handle and troubleshoot the `NotFoundException` to ensure smooth interaction with the AWS Pricing service.

## 6. References
- AWS SDK for Java Documentation: [docs.aws.amazon.com/sdk-for-java](https://docs.aws.amazon.com/sdk-for-java/)
- AWS SDK for Python (Boto3) Documentation: [boto3.amazonaws.com/v1/documentation/api/latest/index.html](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
- AWS CLI Command Reference: [docs.aws.amazon.com/cli](https://docs.aws.amazon.com/cli/index.html)
- AWS Pricing API Documentation: [docs.aws.amazon.com/aws-cost-management/latest/APIReference](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference)