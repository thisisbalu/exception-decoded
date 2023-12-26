---
title: "ContainerNotFoundException in AWS Media Store Data"
date: 2024-02-28 09:00:00 -0000
categories: [AWS, AWS Media Store Data]
tags: [aws, mediastoredata, com.amazonaws.services.mediastoredata.model]
mermaid: true
toc: true
---


AWS Media Store Data is a highly scalable and durable storage service designed specifically for media assets. It provides secure and low-latency storage capabilities, ideal for storing large amounts of media data. However, like any other technology, it is not immune to errors. One such error is the `ContainerNotFoundException` which we will explore in this article.

## What is the ContainerNotFoundException?

The `ContainerNotFoundException` is an exception that occurs when a request is made to access a container that does not exist in AWS Media Store Data. A container in this context represents a storage space for media assets. This exception is thrown when you try to perform operations such as Get, Delete, or PutObject on a non-existent container.

## Why does the ContainerNotFoundException occur?

The primary reason for the `ContainerNotFoundException` is that the container you are trying to access has not been created or has been deleted. It can also occur if you make a typo in the container name or if you are using an incorrect region.

## How to handle the ContainerNotFoundException?

To handle the `ContainerNotFoundException`, you need to ensure that the container you are trying to access exists. You can achieve this by:

1. Confirming the container name: Double-check the container name and ensure that it is spelled correctly. Even a small typo can result in the exception being thrown.

```java
String containerName = "my-container";
```

2. Listing all containers: If you are unsure about the container name or want to verify the existence of a container, you can list all containers using the `ListContainers()` method.

```java
ListContainersRequest request = new ListContainersRequest();
ListContainersResult result = mediaStoreDataClient.listContainers(request);
```

3. Checking for the existence of the container: Once you have the list of containers, you can iterate over them and verify if the container you are looking for exists using its name.

```java
boolean containerExists = false;
for (Container container : result.getContainers()) {
    if (container.getName().equals(containerName)) {
        containerExists = true;
        break;
    }
}
```

4. Handling the ContainerNotFoundException: If the container does not exist, you can handle the exception by either creating the container, informing the user about the non-existent container, or taking any appropriate action depending on your application's requirements.

```java
try {
    // Perform operations on the container
} catch (ContainerNotFoundException ex) {
    // Handle the exception
}
```

## Conclusion

The `ContainerNotFoundException` is a common error that can occur when working with AWS Media Store Data. By ensuring the container existence and taking appropriate actions, you can effectively handle this exception and provide a smooth experience to your users.

For more information on AWS Media Store Data and how to handle exceptions, refer to the [official documentation](https://docs.aws.amazon.com/mediastore/latest/ug/what-is-ms.html).

Remember, double-checking container names and verifying their existence is crucial to avoid the `ContainerNotFoundException`.