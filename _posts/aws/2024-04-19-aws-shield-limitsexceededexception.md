---
title: "How to Handle LimitsExceededException in AWS Shield
            Perform your operation that may result in LimitsExceededException
                ..."
date: 2024-04-19 09:00:00 -0000
categories: [AWS, AWS Shield]
tags: [aws, shield, com.amazonaws.services.shield.model]
mermaid: true
toc: true
---


## Introduction

AWS Shield is a managed Distributed Denial of Service (DDoS) protection service provided by Amazon Web Services (AWS). It helps protect applications and websites from potential DDoS attacks by automatically detecting and mitigating malicious traffic. Shield can be integrated within various AWS services, providing enhanced security for your infrastructure.

In this article, we will focus on the `LimitsExceededException` in the `com.amazonaws.services.shield.model` package of AWS Shield. We'll discuss what this exception represents, when and why it occurs, and how to handle it effectively in your applications.

## What is the `LimitsExceededException`?

The `LimitsExceededException` is an exception class that is part of the AWS Shield API. This exception is thrown when you reach one or more of the AWS Shield service limits, which can restrict your usage or operations. Service limits are safeguards imposed by AWS to ensure fair resource allocation, maintain service performance, and prevent misuse.

## When does the `LimitsExceededException` occur?

The `LimitsExceededException` occurs when you exceed the predefined AWS service limits related to AWS Shield. These limits can vary depending on the characteristics of your AWS account, such as usage patterns, service level agreements, and subscription tier.

For example, you may encounter this exception when trying to create additional DDoS protection policies or enable DDoS protection on new AWS resources, such as Elastic Load Balancers or CloudFront distributions. It can also be triggered when adjusting the rate-based rules or web ACLs associated with AWS Shield Advanced.

## How to Handle the `LimitsExceededException`?

Handling the `LimitsExceededException` effectively is crucial to ensure the smooth operation of your applications and avoid unexpected behavior. Here are three recommended approaches to deal with this exception:

### 1. Monitor and Alert on Service Limits

To proactively handle the `LimitsExceededException`, it is essential to monitor your service limits and set up alerting mechanisms. AWS provides CloudWatch, a monitoring service, which allows you to define custom metrics and alarms based on thresholds. By monitoring Shield service limits, you can receive alarms whenever you approach or breach those predefined limits.

In this code example, we use the AWS SDK for Java to retrieve and monitor the DDoS protection limits:

```java
import com.amazonaws.services.shield.AWSShield;
import com.amazonaws.services.shield.AWSShieldClientBuilder;
import com.amazonaws.services.shield.model.*;

public class ShieldLimitMonitor {

    private static final String REGION = "us-west-2"; // Specify your AWS region

    public static void main(String[] args) {

        AWSShield shieldClient = AWSShieldClientBuilder.standard()
                .withRegion(REGION)
                .build();

        GetSubscriptionStateResult subscriptionStateResult = shieldClient.getSubscriptionState(new GetSubscriptionStateRequest());
        if (subscriptionStateResult.getSubscriptionState().equals(SubscriptionState.ENABLED.toString())) {
            DescribeProtectionLimitsResult protectionLimitsResult = shieldClient.describeProtectionLimits(new DescribeProtectionLimitsRequest());
            for (Limit limit : protectionLimitsResult.getLimits()) {
                System.out.println(limit.getName() + ": " + limit.getMax());
                // Set up CloudWatch alarms based on these limits
            }
        } else {
            System.out.println("AWS Shield subscription is not enabled.");
        }
    }
}
```

By running this code periodically and configuring CloudWatch alarms based on the Shield service limits, you can be notified when certain thresholds are reached or crossed.

### 2. Implement Retry and Backoff Mechanisms

When you encounter the `LimitsExceededException`, it may not always be feasible to immediately reduce usage or apply for service limit increases. In such cases, implementing retry and backoff mechanisms can help manage the situation effectively.

Here is an example of retry logic using the AWS SDK for Python (Boto3):

```python
import time
import boto3
from botocore.exceptions import ClientError

def my_function():
    shield_client = boto3.client('shield', region_name='us-west-2') # Specify your AWS region

    while True:
        try:
            response = shield_client.create_protection(
            )
            break  # Operation succeeded, break out of the loop

        except shield_client.exceptions.LimitsExceededException as e:
            print("Service limit exceeded. Retrying after 5 seconds...")
            time.sleep(5)  # Wait for 5 seconds before retrying

        except ClientError as e:
            print("An error occurred:", e)
            break  # Handle other exceptions

my_function()
```

By using a combination of while-loop, sleep, and exception handling, you can implement custom retry and backoff logic when faced with `LimitsExceededException` in your Python applications.

### 3. Request a Service Limit Increase

If you consistently encounter the `LimitsExceededException` and it affects your application's functionality, requesting a service limit increase is one possible solution. AWS provides a support ticket system where you can submit reliable information to justify your increase request. You should be prepared to specify the limit you would like to increase, as well as any relevant usage metrics or projected growth.

To request a service limit increase, follow these steps:

1. Open the AWS Support Center console.
2. Choose "Create case" to create a new support case.
3. Select the category "Limits and quotas" and the appropriate service (Shield).
4. Provide accurate and complete information about your service limit request.
5. Submit the case and wait for a response from AWS Support.

Remember to provide a detailed explanation of your limits increase requirement, emphasizing the potential impact on your application's functionality or performance.

## Conclusion

In this article, we have explored the `LimitsExceededException` in the `com.amazonaws.services.shield.model` package of AWS Shield. We discussed what this exception represents, why it occurs, and how to handle it effectively in your applications. By monitoring service limits, implementing retry mechanisms, and requesting limit increases when necessary, you can ensure the smooth operation of your applications with AWS Shield.

AWS Shield is an essential tool for protecting your applications and websites against DDoS attacks. Stay vigilant, keep an eye on your service limits, and take the necessary actions to mitigate potential risks.

To learn more about AWS Shield and service limits, refer to the official AWS documentation:

- [AWS Shield documentation](https://aws.amazon.com/shield/)
- [AWS Shield FAQ](https://aws.amazon.com/shield/faqs/)

Happy coding with AWS Shield!