---
title: "Title: Demystifying the ProvisionedConcurrencyConfigNotFoundException in AWS Lambda"
date: 2024-04-28 09:00:00 -0000
categories: [AWS, AWS Lambda]
tags: [aws, lambda, com.amazonaws.services.lambda.model]
mermaid: true
toc: true
---


## Introduction:

In the vast world of AWS Lambda, developers often come across various errors and exceptions while building serverless applications. One such common encounter is the `ProvisionedConcurrencyConfigNotFoundException`. This article aims to provide a comprehensive understanding of this error and offers practical solutions to overcome it.

## Table of Contents:

1. [Understanding Provisioned Concurrency](#understanding-provisioned-concurrency)
2. [What is ProvisionedConcurrencyConfigNotFoundException?](#what-is-provisionedconcurrencyconfignotfoundexception)
3. [Root Causes](#root-causes)
    - [Incorrect Function Name](#incorrect-function-name)
    - [Misconfiguration of Provisioned Concurrency](#misconfiguration-of-provisioned-concurrency)
4. [Troubleshooting ProvisionedConcurrencyConfigNotFoundException](#troubleshooting-provisionedconcurrencyconfignotfoundexception)
   - [1. Verify the Function Name](#1-verify-the-function-name)
   - [2. Check Provisioned Concurrency Configuration](#2-check-provisioned-concurrency-configuration)
   - [3. Grant Sufficient IAM Permissions](#3-grant-sufficient-iam-permissions)
   - [4. Testing Locally](#4-testing-locally)
   - [5. AWS Support](#5-aws-support)
5. [Conclusion](#conclusion)
6. [References](#references)

## Understanding Provisioned Concurrency:

AWS Lambda offers two runtime options for scaling: *On-Demand* and *Provisioned*. While *On-Demand* scales automatically based on the incoming requests, the *Provisioned* concurrency model allows developers to set a predefined number of concurrent executions to ensure predictable performance.

Provisioned concurrency can significantly reduce startup latency for your functions, making them suitable for scenarios requiring low latency, such as real-time applications or APIs that experience frequent bursts of traffic.

## What is ProvisionedConcurrencyConfigNotFoundException?

The `ProvisionedConcurrencyConfigNotFoundException` is an exception thrown by the `com.amazonaws.services.lambda.model` class in the AWS SDK for Java. This error typically occurs when AWS Lambda cannot find the provisioned concurrency configuration for a specified Lambda function.

## Root Causes:

### Incorrect Function Name:

One common root cause for this exception is specifying an incorrect function name. Ensure that you provide the correct function name when requesting the provisioned concurrency configuration.

### Misconfiguration of Provisioned Concurrency:

The provisioned concurrency configuration might not be correctly set up for the Lambda function you are invoking. Revisit the configuration settings to identify and resolve potential misconfigurations.

## Troubleshooting ProvisionedConcurrencyConfigNotFoundException:

When encountering the `ProvisionedConcurrencyConfigNotFoundException`, follow these troubleshooting steps:

### 1. Verify the Function Name:

Double-check the function name you have provided. Ensure that the function name is correct and accurately matches the one specified in the AWS Lambda dashboard or programmatically.

```java
// Example Code:
public class LambdaInvoker {

    private static final String FUNCTION_NAME = "myLambdaFunction";

    public void invokeLambdaFunction() {
        try {
            AWSLambda lambdaClient = AWSLambdaClientBuilder.defaultClient();
            GetProvisionedConcurrencyConfigRequest request = new GetProvisionedConcurrencyConfigRequest()
                            .withFunctionName(FUNCTION_NAME);
            GetProvisionedConcurrencyConfigResult result = lambdaClient.getProvisionedConcurrencyConfig(request);
        } catch (ProvisionedConcurrencyConfigNotFoundException e) {
            // Handle exception
        }
    }
}
```

### 2. Check Provisioned Concurrency Configuration:

Examine the provisioned concurrency configuration provided for the function. Ensure that the given function has a valid provisioned concurrency configuration set up.

```java
// Example Code:
public class ProvisionedConcurrencyConfig {

    private static final String FUNCTION_NAME = "myLambdaFunction";
    private static final int PROVISIONED_CONCURRENCY_COUNT = 10;

    public void setProvisionedConcurrencyConfig() {
        try {
            AWSLambda lambdaClient = AWSLambdaClientBuilder.defaultClient();
            ProvisionedConcurrencyConfigListItem configListItem = new ProvisionedConcurrencyConfigListItem()
                            .withFunctionArn(getFunctionArn())
                            .withCreatedAt(new Date())
                            .withRequestedProvisionedConcurrentExecutions(PROVISIONED_CONCURRENCY_COUNT);
            lambdaClient.putProvisionedConcurrencyConfig(configListItem);
        } catch (Exception e) {
            // Handle exception
        }
    }

    private String getFunctionArn() {
        // Get function ARN programmatically or hardcode if needed
        return "arn:aws:lambda:<Region>:<Account-ID>:function:" + FUNCTION_NAME;
    }
}
```

### 3. Grant Sufficient IAM Permissions:

Ensure that the IAM user or role executing the Lambda function has appropriate permissions to access and manage provisioned concurrency configurations. The policy applied should include the necessary actions on the Lambda service.

### 4. Testing Locally:

To ease debugging, test your Lambda function locally with provisioned concurrency. Tools like [SAM CLI](https://aws.amazon.com/serverless/sam/), [Localstack](https://localstack.cloud/), or [AWS Toolkit for JetBrains](https://www.jetbrains.com/aws/) can be helpful in emulating AWS Lambda environments locally.

### 5. AWS Support:

Should you exhaust all options for resolving the `ProvisionedConcurrencyConfigNotFoundException`, consider reaching out to AWS Support for further assistance. Provide detailed information about your configuration and steps taken for troubleshooting to expedite the process.

## Conclusion:

The `ProvisionedConcurrencyConfigNotFoundException` is an error encountered while fetching provisioned concurrency configurations of an AWS Lambda function. By verifying the function name, checking provisioned concurrency configuration, and ensuring sufficient IAM permissions, you can effectively troubleshoot and rectify this issue. Testing locally and leveraging AWS Support when needed can assist in resolving any persistent challenges.

Enhancing your understanding of provisioned concurrency and its exception handling ensures a smooth and optimized experience while building real-time applications and APIs on AWS Lambda.

## References:

- [AWS Lambda Provisioned Concurrency](https://aws.amazon.com/lambda/features/provisioned-concurrency/)
- [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/)
- [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

---

*This article is a 15-minute read and is intended to provide a detailed solution for the ProvisionedConcurrencyConfigNotFoundException in AWS Lambda.