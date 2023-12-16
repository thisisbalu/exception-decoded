---
title: "Catchy and SEO Friendly Title: Understanding AutomationDefinitionNotApprovedException in AWS SSM"
date: 2023-12-24 09:00:00 -0000
categories: [AWS, AWS Simple Systems Management (SSM)]
tags: [aws, simplesystemsmanagement, com.amazonaws.services.simplesystemsmanagement.model]
mermaid: true
toc: true
---

## Introduction

In the realm of AWS Simple Systems Management (SSM), automation workflows enhance operational efficiency and enable infrastructure management at scale. However, while utilizing the power of automation, developers and system administrators might encounter exceptions. One such exception is `AutomationDefinitionNotApprovedException`. In this article, we will delve into what this exception signifies, its possible causes, and how to handle it effectively.

## Understanding AutomationDefinitionNotApprovedException

The `AutomationDefinitionNotApprovedException` is a specific exception class present in the `com.amazonaws.services.simplesystemsmanagement.model` package of AWS SSM. This exception occurs when attempting to execute an automation workflow that has not been approved by the AWS account owner or an approved SSM master account.

## Possible Causes

There are several reasons why the `AutomationDefinitionNotApprovedException` might arise. Let's take a look at a few common scenarios:

### 1. Automation Approval Not Granted

One of the most common causes of this exception is when the automation workflow has not been approved by the appropriate parties. The AWS account owner or an approved SSM master account must explicitly approve the automation document before it can be executed successfully.

To check if a specific automation document has been approved, you can use the `describeDocument()` method from the `AWSSimpleSystemsManagement` client class. If the document status is `APPROVED`, the automation workflow has been approved and can be executed without encountering this exception.

```java
import com.amazonaws.services.simplesystemsmanagement.AWSSimpleSystemsManagement;
import com.amazonaws.services.simplesystemsmanagement.model.DescribeDocumentRequest;
import com.amazonaws.services.simplesystemsmanagement.model.DescribeDocumentResult;

AWSSimpleSystemsManagement ssmClient = AWSSimpleSystemsManagementClientBuilder.standard().build();
DescribeDocumentRequest describeRequest = new DescribeDocumentRequest().withName("MyAutomationWorkflow");
DescribeDocumentResult describeResponse = ssmClient.describeDocument(describeRequest);
String documentStatus = describeResponse.getStatus();
if (documentStatus.equals("APPROVED")) {
  // Automation workflow approved, proceed with execution
} else {
  throw new AutomationDefinitionNotApprovedException("Automation workflow not approved.")
}
```

### 2. Insufficient IAM Permissions

Another potential cause of the `AutomationDefinitionNotApprovedException` is the lack of appropriate IAM permissions required to execute an automation workflow. To resolve this, ensure that the IAM role associated with the entity executing the automation workflow has the necessary permissions.

Typically, the IAM policy must contain the `ssm:SendCommand` action, allowing the entity to send commands for executing the automation document. Additionally, if the automation workflow uses AWS Systems Manager Parameter Store parameters, the role must also include permissions like `ssm:GetParameters` to fetch the required parameters.

### 3. Automation Document Not Found

If you encounter the `AutomationDefinitionNotApprovedException` and suspect that the automation document might not exist, you can verify its existence by calling the `describeDocument()` method, as shown below:

```java
if (!doesDocumentExist("MyAutomationWorkflow")) {
  throw new AutomationDefinitionNotApprovedException("Automation workflow not found.");
}

// Method implementation to check if a document exists
public boolean doesDocumentExist(String documentName) {
  List<DocumentIdentifier> documents = ssmClient.listDocuments().getDocumentIdentifiers();
  return documents.stream().anyMatch(document -> document.getName().equals(documentName));
}
```

## Handling AutomationDefinitionNotApprovedException

To handle the `AutomationDefinitionNotApprovedException` effectively, follow these steps:

1. Ensure the automation workflow has been approved by the appropriate parties.
2. Verify that the executing entity and IAM role have the necessary permissions to execute the automation document.
3. Check if the automation document exists before attempting to execute it.

By adhering to these steps, you can minimize the occurrence of the `AutomationDefinitionNotApprovedException` and enable seamless automation workflows within your AWS SSM environment.

## Conclusion

Automation workflows in AWS SSM provide powerful capabilities for managing infrastructure and streamlining operational tasks. However, it is crucial to handle exceptions like `AutomationDefinitionNotApprovedException` appropriately. By understanding the causes and resolving them effectively, you can enhance the reliability and efficiency of your automation workflows.

To learn more about AWS Simple Systems Management and automation workflows, refer to the official AWS documentation:

- [AWS SSM Documentation](https://aws.amazon.com/documentation/systems-manager/)
- [Automation Workflows with AWS SSM](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html)

Thank you for taking the time to read this article!
