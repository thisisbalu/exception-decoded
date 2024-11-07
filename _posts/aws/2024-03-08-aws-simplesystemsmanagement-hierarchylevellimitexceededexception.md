---
title: "Title: Understanding HierarchyLevelLimitExceededException in AWS Simple Systems Management (SSM)"
date: 2024-03-08 09:00:00 -0000
categories: [AWS, AWS Simple Systems Management (SSM)]
tags: [aws, simplesystemsmanagement, com.amazonaws.services.simplesystemsmanagement.model]
mermaid: true
toc: true
---


## Introduction

In AWS Simple Systems Management (SSM), the HierarchyLevelLimitExceededException is a common error that occurs when the number of hierarchy levels for a parameter exceeds the allowed limit. This exception usually arises when managing a large number of parameters with complex organizational hierarchies.

## What is AWS Simple Systems Management (SSM)?

AWS Simple Systems Management (SSM) is a fully-managed service that enables you to manage your infrastructure resources seamlessly. It provides a unified interface to manage, configure, and automate resources across your EC2 instances and on-premises machines. SSM simplifies operational tasks like patch management, software deployment, and state management, saving time and effort for system administrators.

## Understanding Hierarchy Levels in AWS SSM

Hierarchy levels in AWS SSM are used to organize and manage parameters within a hierarchical structure. The hierarchy allows you to logically group parameters based on different properties such as environment, application, or region.

The hierarchy level of a parameter is defined by a forward slash (/) separator. For example, consider the following hierarchy of parameters:

- /production/application1/parameter1
- /production/application1/parameter2
- /production/application2/parameter1

Here, "production" is the top-level hierarchy, followed by "application1" and "application2" as sub-levels. Finally, we have individual parameters like "parameter1" and "parameter2" at the lowest level. You can have multiple hierarchy levels depending on your organization's requirements.

## HierarchyLevelLimitExceededException - Understanding the Error

The HierarchyLevelLimitExceededException is thrown when the number of hierarchy levels for a parameter exceeds the maximum limit set by AWS SSM. By default, the maximum allowed number of hierarchy levels is 15.

When this exception occurs, it indicates that you have reached the maximum limit for hierarchy levels and cannot add any more levels to the hierarchy. It's important to address this exception to ensure the proper organization and management of your parameters.

## Resolving HierarchyLevelLimitExceededException

To resolve the HierarchyLevelLimitExceededException, you have the following options:

### 1. Reorganize the Hierarchy

One approach is to reorganize the existing hierarchy by merging or eliminating some hierarchy levels. Analyze the current hierarchy structure and identify if any levels can be combined or removed without losing the required organization. This can help reduce the hierarchy level count and avoid reaching the maximum limit.

### 2. Shorten Parameter Names

Another way to tackle the exception is by shortening the names of the parameters themselves. By minimizing the length of parameter names, you can potentially decrease the number of levels required within the hierarchy. This approach allows more flexibility in managing parameters without exceeding the hierarchy level limit.

### 3. Utilize AWS SSM Parameter Store Structure

AWS SSM Parameter Store supports a hierarchy-free structure using path-based naming conventions. Instead of relying solely on hierarchy levels, you can organize parameters using a forward slash ("/") as a separator. This approach eliminates the constraints of hierarchy levels and allows for an unlimited number of parameters.

Here's an example of creating a parameter without hierarchy levels using the AWS CLI:

```bash
aws ssm put-parameter \
  --name /production/application1/parameter1 \
  --value "example-value" \
  --type String
```

## Conclusion

The HierarchyLevelLimitExceededException is an important error to address when dealing with a large number of parameters in AWS Simple Systems Management. By understanding the meaning of this exception and implementing the suggested resolutions, you can effectively manage your parameters within the allowed hierarchy levels.

Being aware of the limit of hierarchy levels, and knowing how to organize your parameters efficiently, ensures a smooth experience while maintaining and configuring your resources using AWS SSM.

To learn more about AWS Simple Systems Management and parameter management, refer to the following resources:

- [AWS SSM Documentation](https://docs.aws.amazon.com/systems-manager/index.html)
- [AWS CLI Command Reference for SSM](https://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)
- [AWS SSM API Reference](https://docs.aws.amazon.com/ssm/latest/APIReference/Welcome.html)

Remember to optimize your hierarchy, utilize path-based naming, and keep an eye on the hierarchy level limits to harness the full potential of AWS SSM.

Keep exploring and simplifying your infrastructure management with AWS Simple Systems Management!