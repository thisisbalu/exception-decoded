---
title: "EntityNotExistsException in AWS WorkDocs"
date: 2024-06-05 09:00:00 -0000
categories: [AWS, AWS WorkDocs]
tags: [aws, workdocs, com.amazonaws.services.workdocs.model]
mermaid: true
toc: true
---


---

## Introduction

**EntityNotExistsException** is an exception class in the **com.amazonaws.services.workdocs.model** package of the AWS WorkDocs SDK. It is thrown when the requested entity does not exist in the AWS WorkDocs service. In this article, we will explore the details of the EntityNotExistsException and how to handle it effectively.

## Understanding EntityNotExistsException

When working with the AWS WorkDocs service, there are various operations that can be performed, such as creating a folder, uploading a document, or sharing a document. Each of these operations involves working with entities like folders and documents. The EntityNotExistsException is thrown when we try to perform an operation on an entity that does not exist.

For example, if we try to get the metadata of a non-existent document using the `GetDocument` operation, the EntityNotExistsException will be thrown.

## Handling EntityNotExistsException

To handle the EntityNotExistsException effectively, we need to first catch the exception and then implement the necessary error handling logic. Here's an example of how to handle the EntityNotExistsException in Java:

```java
try {
    // Perform operation on entity
} catch (EntityNotExistsException ex) {
    // Handle entity not found error
    System.out.println("The requested entity does not exist.");
}
```

In the above code, we catch the EntityNotExistsException and print a user-friendly error message. Depending on the requirements of your application, you can choose to log the error, display it to the user, or take any other appropriate action.

## Common Scenarios

### Scenario 1: Retrieving a non-existent document

Let's say we want to retrieve the metadata of a document using the `GetDocument` operation. If the document does not exist, the EntityNotExistsException will be thrown. Here's an example code snippet:

```java
public DocumentMetadata getDocumentMetadata(String documentId) {
    GetDocumentRequest request = new GetDocumentRequest().withDocumentId(documentId);
    
    try {
        GetDocumentResult result = workDocsClient.getDocument(request);
        return result.getMetadata();
    } catch (EntityNotExistsException ex) {
        // Handle entity not found error
        System.out.println("The requested document does not exist.");
        return null;
    }
}
```

### Scenario 2: Moving a non-existent folder

Suppose we want to move a folder from one location to another using the `UpdateDocumentVersion` operation. If the source or destination folders do not exist, the EntityNotExistsException will be thrown. Here's an example code snippet:

```java
public void moveFolder(String folderId, String destinationFolderId) {
    UpdateDocumentVersionRequest request = new UpdateDocumentVersionRequest()
            .withDocumentId(folderId)
            .withVersionId("1")
            .withParentFolderId(destinationFolderId);
            
    try {
        UpdateDocumentVersionResult result = workDocsClient.updateDocumentVersion(request);
        // Handle success
    } catch (EntityNotExistsException ex) {
        // Handle entity not found error
        System.out.println("One or both folders do not exist.");
    }
}
```

## Conclusion

In this article, we have explored the EntityNotExistsException in AWS WorkDocs. We discussed how this exception is thrown when the requested entity does not exist. We also learned how to handle the exception effectively by implementing the necessary error handling logic.

By understanding and properly handling the EntityNotExistsException, you can improve the robustness and reliability of your AWS WorkDocs applications.

To learn more about the EntityNotExistsException and other exceptions in AWS WorkDocs, refer to the [AWS WorkDocs API Reference](https://docs.aws.amazon.com/workdocs/latest/APIReference/API_Operations.html).

Happy coding!

---
**Note**: This article is a part of a series on AWS WorkDocs exceptions. Check out our other articles on WorkDocsException and UnauthorizedOperationException for a comprehensive understanding of handling exceptions in AWS WorkDocs.