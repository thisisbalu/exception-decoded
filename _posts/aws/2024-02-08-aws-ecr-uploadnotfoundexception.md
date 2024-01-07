---
title: "Uploading Images to AWS Elastic Container Registry: Understanding the UploadNotFoundException"
date: 2024-02-08 09:00:00 -0000
categories: [AWS, AWS Elastic Container Registry]
tags: [aws, ecr, com.amazonaws.services.ecr.model]
mermaid: true
toc: true
---


If you have ever worked with containerization in AWS, you must be familiar with AWS Elastic Container Registry (ECR). ECR is a fully-managed container registry service that makes it easy to store, manage, and deploy container images. While working with ECR, you might come across an error called "UploadNotFoundException". In this article, we will explore what this error means, its potential causes, and how to troubleshoot it.

## What is the UploadNotFoundException?

The UploadNotFoundException is an exception thrown by the `com.amazonaws.services.ecr.model` class in the AWS SDK. It indicates that the specified upload was not found in the ECR registry. When you attempt to upload a container image to ECR, the upload process typically involves a series of steps, such as initiating the upload, uploading the image layers, and finalizing the upload. The UploadNotFoundException occurs when ECR cannot find the specified upload during any of these steps.

## Potential Causes

There can be several reasons why the UploadNotFoundException is thrown. Let's explore some of the common causes:

### 1. Expired Upload URL

When you initiate an upload to ECR, you receive a pre-signed URL that is valid for a specific period. If you do not complete the upload within the validity period, the upload URL expires. Any attempt to finalize or upload layers using an expired URL will result in the UploadNotFoundException. Make sure you complete the upload process before the URL expiration time.

```java
import com.amazonaws.services.ecr.AmazonECR;
import com.amazonaws.services.ecr.model.*;
import com.amazonaws.services.ecr.AmazonECRClientBuilder;

public class ECRUploader {
    private AmazonECR ecrClient;

    public ECRUploader() {
        ecrClient = AmazonECRClientBuilder.defaultClient();
    }

    public void uploadImage() {
        InitiateLayerUploadRequest initiateRequest = new InitiateLayerUploadRequest()
                .withRepositoryName("my-repo")
                .withRegistryId("123456789012");

        InitiateLayerUploadResult response = ecrClient.initiateLayerUpload(initiateRequest);
        
        // Store the `uploadId` and `uploadUrl` received in response for later use
        String uploadId = response.getUploadId();
        String uploadUrl = response.getUploadUrl();
        
        // Upload the image layers using the uploadUrl
        
        // Finalize the upload
        CompleteLayerUploadRequest completeRequest = new CompleteLayerUploadRequest()
                .withRepositoryName("my-repo")
                .withRegistryId("123456789012")
                .withUploadId(uploadId);

        // Use the expired uploadUrl below, intentionally for demonstration purposes
        completeRequest.setUploadEndpoints(Arrays.asList(uploadUrl)); 

        try {
            CompleteLayerUploadResult completeResponse = ecrClient.completeLayerUpload(completeRequest);
        } catch (UploadNotFoundException e) {
            // Handle the UploadNotFoundException
        }
    }
}
```

### 2. Invalid Repository Name or Registry ID

Another possible cause of the UploadNotFoundException is providing an incorrect repository name or registry ID in API requests. Double-check the repository name and registry ID you are using and ensure they match the actual values in your ECR instance.

```java
// ...
// Initialize the ECR client: ecrClient

String invalidRepoName = "non-existent-repo";

// Use an invalid repository name intentionally for demonstration purposes
InitiateLayerUploadRequest initiateRequest = new InitiateLayerUploadRequest()
        .withRepositoryName(invalidRepoName)
        .withRegistryId("123456789012");

try {
    InitiateLayerUploadResult response = ecrClient.initiateLayerUpload(initiateRequest);
} catch (RepositoryNotFoundException e) {
    // Handle the RepositoryNotFoundException
}
```

### 3. Incorrect Access Permissions

The UploadNotFoundException can also occur due to incorrect access permissions. Ensure that the IAM user or role used to perform the ECR operations has the necessary permissions like `ecr:InitiateLayerUpload`, `ecr:UploadLayerPart`, and `ecr:CompleteLayerUpload`. Verify the IAM policies associated with the user or role and make sure they grant the required permissions.

```java
// ...
// Assume the IAM role or user associated with this client has insufficient permissions

InitiateLayerUploadRequest initiateRequest = new InitiateLayerUploadRequest()
        .withRepositoryName("my-repo")
        .withRegistryId("123456789012");

try {
    InitiateLayerUploadResult response = ecrClient.initiateLayerUpload(initiateRequest);
} catch (AmazonECRException e) {
    // Handle the exception related to insufficient permissions
}
```

## Troubleshooting Steps

If you encounter the UploadNotFoundException, follow these troubleshooting steps:

1. Double-check the validity of the upload URL and ensure it has not expired.
2. Verify that the repository name and registry ID used in the API requests are correct.
3. Review the permissions of the IAM user or role used for performing ECR operations.
4. Check if there are any network connectivity issues that could prevent the upload process.

By following these steps, you can identify and resolve the issues causing the UploadNotFoundException.

## Conclusion

In this article, we explored the UploadNotFoundException, an error that can occur during the image upload process to AWS Elastic Container Registry. We discussed its potential causes, including expired upload URLs, incorrect repository names or registry IDs, and incorrect access permissions. We also provided code examples demonstrating how to handle these scenarios.

Understanding and troubleshooting the UploadNotFoundException will help you ensure a smooth uploading experience when working with AWS Elastic Container Registry.

For more information, refer to the official [AWS Elastic Container Registry documentation](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/Welcome.html).

Happy containerizing!