---
title: "AutomationExecutionNotFoundException in AWS SSM: A Detailed Guide"
date: 2023-12-26 09:00:00 -0000
categories: [AWS, AWS Simple Systems Management (SSM)]
tags: [aws, simplesystemsmanagement, com.amazonaws.services.simplesystemsmanagement.model]
mermaid: true
toc: true
---


Have you ever encountered the AutomationExecutionNotFoundException error while working with AWS Simple Systems Management (SSM)? If so, you're not alone. This error can be quite frustrating, especially if you're new to AWS SSM. But fret not! In this comprehensive guide, we will explore this error in detail and discuss how to troubleshoot and resolve it effectively.

## Table of Contents
1. Introduction
2. Understanding AutomationExecutionNotFoundException
3. Troubleshooting Steps
4. Resolving AutomationExecutionNotFoundException
5. Conclusion

## 1. Introduction

AWS Simple Systems Management (SSM) is a powerful service that helps you manage and configure your EC2 instances and on-premises systems at scale. It provides a unified user interface and API to manage your resources, automate tasks, and ensure compliance across your entire infrastructure.

While working with AWS SSM, you may come across various exceptions and errors, one of which is the AutomationExecutionNotFoundException.

## 2. Understanding AutomationExecutionNotFoundException

The AutomationExecutionNotFoundException is an exception that occurs when you try to access an automation execution that does not exist. This exception is thrown by the `com.amazonaws.services.simplesystemsmanagement.model` class in the Amazon Web Services SDK for Java.

### Example Code:

To better understand this exception, let's consider an example where we try to describe an automation execution:

```java
import com.amazonaws.services.simplesystemsmanagement.AWSSimpleSystemsManagement;
import com.amazonaws.services.simplesystemsmanagement.AmazonSSMClientBuilder;
import com.amazonaws.services.simplesystemsmanagement.model.AutomationExecutionNotFoundException;
import com.amazonaws.services.simplesystemsmanagement.model.DescribeAutomationExecutionsRequest;
import com.amazonaws.services.simplesystemsmanagement.model.DescribeAutomationExecutionsResult;

public class SSMExample {

    public static void main(String[] args) {
        AWSSimpleSystemsManagement ssmClient = AmazonSSMClientBuilder.standard().build();
        
        DescribeAutomationExecutionsRequest request = new DescribeAutomationExecutionsRequest()
                .withFilters(/* Add filters as required */);
        
        try {
            DescribeAutomationExecutionsResult result = ssmClient.describeAutomationExecutions(request);
            
            // Process the automation executions here
            
        } catch (AutomationExecutionNotFoundException e) {
            System.out.println("Automation execution not found: " + e.getMessage());
        }
    }
}
```

In this code snippet, we create an SSM client and try to describe automation executions using the `describeAutomationExecutions` method. If an execution is not found, an AutomationExecutionNotFoundException is thrown.

## 3. Troubleshooting Steps

Encountering the AutomationExecutionNotFoundException can be frustrating, but there are steps you can take to troubleshoot the issue effectively. Here are some suggestions to help you get started:

### 3.1 Verify Automation Execution ID

The first step is to ensure that you are providing the correct automation execution ID. Double-check the ID you are passing as a parameter and verify if it exists.

### 3.2 Check Execution Status

Another common reason for the AutomationExecutionNotFoundException is that the execution may have already completed or terminated. Check the status of the execution using the `describeAutomationExecutions` method and validate if it still exists.

### 3.3 Verify Permissions and Access

Ensure that the IAM role associated with your AWS SSM client has sufficient permissions to access and describe automation executions. It should include actions like `ssm:DescribeAutomationExecutions` to interact with automation executions.

## 4. Resolving AutomationExecutionNotFoundException

Once you have identified the cause of the AutomationExecutionNotFoundException, resolving it becomes relatively straightforward. Here are some possible solutions:

### 4.1 Validate the Input

Double-check the automation execution ID and confirm that it is correct. If it is not, update the ID and re-run your code.

### 4.2 Handle Exception Gracefully

Consider implementing appropriate error handling logic to gracefully handle the AutomationExecutionNotFoundException. This will allow your application to handle the exception and provide a meaningful response to the user.

## 5. Conclusion

In this article, we explored the AutomationExecutionNotFoundException in AWS Simple Systems Management (SSM). We discussed its causes, troubleshooting steps, and effective solutions to resolve the issue. By following the suggestions outlined in this guide, you can quickly overcome this exception and continue working seamlessly with AWS SSM.

To learn more about AWS SSM and its capabilities, refer to the [official AWS SSM documentation](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html).

Thank you for reading, and happy troubleshooting!