---
title: "Understanding CloudWatchLogsDeliveryUnavailableException in AWS CloudTrail"
date: 2024-10-03 09:00:00 -0000
categories: [AWS, AWS CloudTrail]
tags: [aws, cloudtrail, com.amazonaws.services.cloudtrail.model]
mermaid: true
toc: true
---


AWS CloudTrail is a vital service for monitoring and auditing your AWS account activities. However, while using CloudTrail, developers may encounter exceptions, one of which is `CloudWatchLogsDeliveryUnavailableException`. This article provides a comprehensive dive into this exception, its causes, and how to handle it effectively.

## What is CloudWatchLogsDeliveryUnavailableException?

`CloudWatchLogsDeliveryUnavailableException` is an exception thrown by the AWS SDK when CloudTrail is unable to deliver logs to AWS CloudWatch Logs. This can be due to several reasons, such as lack of necessary permissions, misconfigured log groups, or insufficient resources in your AWS account.

### When Does It Occur?

This exception can occur in various scenarios, including:
- When the CloudTrail service tries to deliver logs but encounters issues with the CloudWatch Logs service.
- If the log group specified in the CloudTrail settings does not exist.
- When the IAM policy does not grant CloudTrail sufficient permissions to write logs to CloudWatch.

Understanding this exception is crucial for effective cloud management and ensuring that necessary logs are being captured and stored for auditing and monitoring purposes.

## Causes of CloudWatchLogsDeliveryUnavailableException

1. **Log Group Does Not Exist**:
   If the log group specified in the CloudTrail configuration does not exist in CloudWatch Logs, you will encounter this exception.

2. **IAM Permissions Issues**:
   CloudTrail needs specific permissions to write logs to CloudWatch Logs. If these permissions are missing, the delivery will fail, leading to this exception.

3. **CloudWatch Logs Service Issues**:
   Temporary issues with the CloudWatch Logs service may also cause this exception. This could include throttling, service outages, or API rate limits being exceeded.

4. **Resource Limits**:
   AWS imposes limits on the number of log streams and other related resources in CloudWatch. Exceeding these limits can also trigger this exception.

## How to Handle CloudWatchLogsDeliveryUnavailableException

### Step 1: Check the Log Group Configuration

Ensure that the log group specified in your CloudTrail settings exists. You can use the AWS Management Console or the AWS CLI to verify.

**AWS CLI Example**:
```bash
aws logs describe-log-groups --log-group-name "your-log-group-name"
```

### Step 2: Validate IAM Permissions

Make sure that the IAM role used by CloudTrail has the necessary permissions to write to CloudWatch Logs. The policy should include at least the following actions:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        }
    ]
}
```

To attach the policy, you can use the AWS Management Console or the following AWS CLI command:
```bash
aws iam put-role-policy --role-name your-role-name --policy-name CloudTrailLogsPolicy --policy-document file://policy.json
```

### Step 3: Monitor CloudWatch Service Health

Check the [AWS Service Health Dashboard](https://status.aws.amazon.com/) for any outages or issues with CloudWatch Logs. If there are ongoing issues, you might have to wait until they are resolved.

### Step 4: Review Resource Limits

Confirm that you are not exceeding the AWS account limits related to CloudWatch Logs. You can check current limits and request increases via the AWS Support Center if needed.

### Step 5: Implement Retry Logic

Since this exception can sometimes be transient, implementing a retry mechanism can help. In Java, you might handle exceptions as follows:

```java
import com.amazonaws.services.cloudtrail.AWSCloudTrail;
import com.amazonaws.services.cloudtrail.AWSCloudTrailClientBuilder;
import com.amazonaws.services.cloudtrail.model.CloudWatchLogsDeliveryUnavailableException;
import com.amazonaws.services.cloudtrail.model.StartLoggingRequest;

public class CloudTrailLogSender {

    private final AWSCloudTrail cloudTrail;

    public CloudTrailLogSender() {
        this.cloudTrail = AWSCloudTrailClientBuilder.defaultClient();
    }

    public void startLogging(String trailName) {
        int attempts = 0;
        boolean success = false;

        while (attempts < 3 && !success) {
            try {
                StartLoggingRequest request = new StartLoggingRequest().withName(trailName);
                cloudTrail.startLogging(request);
                success = true; // Logging started successfully
            } catch (CloudWatchLogsDeliveryUnavailableException e) {
                System.err.println("Failed to deliver logs, attempt " + (attempts + 1));
                attempts++;
                waitBeforeRetry(); // Implement your backoff logic here
            }
        }

        if (!success) {
            System.err.println("Failed to start logging after multiple attempts.");
        }
    }

    private void waitBeforeRetry() {
        try {
            Thread.sleep(2000); // Wait for 2 seconds before retrying
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

## Conclusion

Handling `CloudWatchLogsDeliveryUnavailableException` effectively ensures that your AWS CloudTrail logs are properly captured in CloudWatch Logs. By following the steps outlined above, including checking configurations, validating IAM policies, monitoring service health, reviewing resource limits, and implementing retry logic, you can mitigate potential issues and maintain robust logging practices.

### References

- [AWS CloudTrail Documentation](https://docs.aws.amazon.com/cloudtrail/index.html)
- [AWS CloudWatch Logs - What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)
- [AWS IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)

By understanding and addressing `CloudWatchLogsDeliveryUnavailableException`, you can enhance your AWS monitoring and auditing capabilities while ensuring the integrity of your logging infrastructure.