---
title: "Title: Demystifying AutomationDefinitionNotApprovedException in AWS Simple Systems Management (SSM)"
date: 2023-12-24 09:00:00 -0000
categories: [AWS, AWS Simple Systems Management (SSM)]
tags: [aws, simplesystemsmanagement, com.amazonaws.services.simplesystemsmanagement.model]
mermaid: true
toc: true
---


## Introduction

In the ever-evolving world of cloud computing, automation has become the holy grail for efficient operations. AWS Simple Systems Management (SSM) provides a comprehensive suite of tools to manage and automate infrastructure at scale. However, while working with SSM Automation, you might encounter an exception called `AutomationDefinitionNotApprovedException`. In this article, we will delve into the details of this exception and explore ways to mitigate it.

## What is AutomationDefinitionNotApprovedException?

`AutomationDefinitionNotApprovedException` is an exception that occurs when attempting to execute an automation document that awaits manual approval. This exception is specific to SSM Automation, which allows you to create workflows for performing complex operational tasks across your infrastructure.

## Understanding SSM Automation Workflow

Before we dive deeper into the exception, let's gain a high-level understanding of how SSM Automation works. The workflow typically consists of the following steps:

1. **Automation Document Creation**: An automation document is authored using either the JSON or YAML format. It defines the sequence of steps and actions to be performed. These documents can be created from scratch or leveraged from pre-existing templates provided by AWS or the community.

2. **Document Execution**: Once the automation document is created, it can be executed directly from the AWS Management Console, AWS CLI, or programmatically using SDKs. SSM Automation takes care of executing the steps defined in the document and provides logging and error handling mechanisms.

3. **Manual Approval**: In certain scenarios, an automation document might require manual approval before proceeding to the next step. This is useful when performing critical operations or making changes that need human intervention. The document execution pauses at such approval steps and waits for approval from authorized personnel.

4. **Exception Handling**: If an approved automation document encounters an error or fails to complete successfully, an exception is thrown. This exception may vary depending on the nature of the error.

## Common Causes of AutomationDefinitionNotApprovedException

AutomationDefinitionNotApprovedException occurs specifically when an automation document encounters an approval step and awaits manual approval. Below are some common scenarios that can lead to this exception:

1. **Missing Approval**: The most straightforward cause of this exception is when an required approval is not provided. If the automation document specifies that an approval is necessary, but none is granted, the document execution remains in a waiting state and eventually times out, resulting in the exception.

2. **Unauthorized Approval**: Sometimes, an unauthorized user might attempt to execute an automation document that requires approval from a specific IAM role or group. In such cases, the user does not have the necessary permissions to approve the document execution, leading to the exception.

3. **Approval Timeouts**: Automation documents can be configured with a timeout period for awaiting approval. If the manual approval is not provided within the specified time, the document execution times out and throws AutomationDefinitionNotApprovedException.

## Resolving AutomationDefinitionNotApprovedException

To resolve AutomationDefinitionNotApprovedException, you need to ensure that the automation document progresses by providing the necessary approval. Below are some ways to tackle this exception:

1. **Grant Approval**: If you encounter this exception and have the appropriate permissions, you can manually approve the automation document execution from the AWS Management Console or CLI. This will allow the document to proceed to the next step.

   ```bash
   aws ssm start-automation-execution --document-name "MyAutomationDocument" --parameters '{"param_name":"param_value"}' --approve
   ```

2. **Check Permissions**: If you do not have the necessary permissions to grant the approval, reach out to the authorized personnel who can provide the approval. Ensure that the required IAM roles or groups are defined correctly and that you are a part of the appropriate group.

3. **Increase Timeout Period**: If your automation document has a short timeout period for awaiting approval, you can modify it to provide more time for approval. This can be done by modifying the `TimeoutSeconds` parameter in the automation document definition.

   ```yaml
   ---
   AutomationDocument:
        - Name: MyAutomationDocument
          TimeoutSeconds: 900
          Steps:
            - Name: Step1
              Action: ...
            - Name: Step2
              Action: ...
   ```

4. **Define Error Handling**: To handle scenarios where an approval is not granted within the specified timeout, you can define appropriate error handling mechanisms in your automation document. For example, you can choose to terminate the document execution or send notifications to stakeholders.

## Conclusion

AWS Simple Systems Management (SSM) Automation is a powerful tool to automate your infrastructure management. The AutomationDefinitionNotApprovedException acts as a gatekeeper, keeping important operations on hold until manual approval is granted. By understanding the causes and resolution methods for this exception, you can ensure smoother execution and better control over your automation workflows in SSM.

To learn more about AWS SSM Automation and its capabilities, refer to the official [AWS SSM Automation Documentation](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-state-manager.html).

Happy automating!

*Estimated Reading Time: 15 minutes*