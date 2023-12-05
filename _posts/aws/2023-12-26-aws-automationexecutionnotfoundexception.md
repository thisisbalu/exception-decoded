---
title: "Troubleshooting Automation Execution Not Found Exception in AWS Simple Systems Management (SSM)"
date: 2023-12-26 09:00:00 -0000
categories: [AWS, AWS Simple Systems Management (SSM)]
tags: [aws, simplesystemsmanagement, com.amazonaws.services.simplesystemsmanagement.model]
mermaid: true
toc: true
---


As an AWS Simple Systems Management (SSM) user, you may encounter different exceptions while working with the automation feature. One such exception is the `AutomationExecutionNotFoundException`. This article will dive deep into this exception, explaining its causes, how to troubleshoot it, and provide relevant code examples.

## Introduction to Automation Execution Not Found Exception
When using the Automation feature in AWS SSM, you might come across the `AutomationExecutionNotFoundException`. This exception occurs when an automation execution is not found in the specified AWS account or region. The exception is thrown by the `com.amazonaws.services.simplesystemsmanagement.model` class when a specific automation execution is not located.

## Causes of Automation Execution Not Found Exception
There can be several reasons behind the occurrence of the `AutomationExecutionNotFoundException`. Some common causes are:

1. Invalid Execution ID: The Execution ID used to identify the automation execution might be invalid or incorrect.
2. Deleted Executions: The automation execution might have been deleted or terminated, resulting in the not found exception.
3. Incorrect Automation Document Name: If the automation document name used in the execution does not exist or is misspelled, the exception can be thrown.
4. Different Regions: The execution might be searched for in a different region from where it was initiated, leading to the exception's occurrence.

## Troubleshooting Automation Execution Not Found Exception
To troubleshoot the `AutomationExecutionNotFoundException`, follow these steps:

### 1. Verify Execution ID
Ensure that the Execution ID used to locate the execution is accurate. Double-check the ID, as any typographical errors can lead to the not found exception.

Here's an example of how to retrieve an automation execution by Execution ID using the AWS SDK for Java:

```java
import com.amazonaws.services.simplesystemsmanagement.*;
import com.amazonaws.services.simplesystemsmanagement.model.*;

String executionId = "your_execution_id";
AmazonSSM ssmClient = AmazonSSMClientBuilder.defaultClient();

DescribeAutomationExecutionsRequest request = new DescribeAutomationExecutionsRequest()
    .withFilters(new AutomationExecutionFilter()
        .withKey("ExecutionId")
        .withValues(executionId));

DescribeAutomationExecutionsResult result = ssmClient.describeAutomationExecutions(request);
```

### 2. Check for Deleted Executions
If an automation execution is deleted or terminated, it cannot be found. To confirm if this is the cause, review the automation execution's status history.

Here's an example of how to list automation execution details for a given Execution ID using the AWS CLI:

```
aws ssm list-automation-executions --filter Key=ExecutionId,Values=your_execution_id
```

### 3. Validate Automation Document Name
Ensure that the automation document name provided in the execution request exists and is spelled correctly. Mismatched or incorrect document names can result in the not found exception.

Here's an example of initiating an automation execution using the AWS SDK for Python (Boto3):

```python
import boto3

automation_client = boto3.client('ssm')

document_name = 'your_document_name'
response = automation_client.start_automation_execution(
    DocumentName=document_name
)
```

### 4. Confirm Execution Region
Make sure that the execution is searched for in the same region from where it was initiated. Different regions might result in an execution not found exception.

## Conclusion
The `AutomationExecutionNotFoundException` can occur due to various reasons, such as an invalid Execution ID, deleted executions, incorrect automation document name, or differences in regions. By following the troubleshooting steps provided, you can identify and resolve the issue causing this exception.

To learn more about AWS SSM Automation and its related concepts, refer to the official [AWS Simple Systems Management documentation](https://aws.amazon.com/systems-manager/).

Happy automating in the AWS cloud!