---
title: "ResourceNotFoundException in AWS CodeStar Connections: Troubleshooting and Solutions
Example Python code to create a connection"
date: 2024-06-15 09:00:00 -0000
categories: [AWS, AWS CodeStar Connections]
tags: [aws, codestarconnections, com.amazonaws.services.codestarconnections.model]
mermaid: true
toc: true
---


When using AWS CodeStar Connections, you may encounter exceptions like the `ResourceNotFoundException` when interacting with its APIs. This error signifies that a requested resource within the CodeStar Connections service was not found. In this article, we will dive deeper into understanding this exception and explore ways to troubleshoot and resolve it in your applications.

## What is AWS CodeStar Connections?
AWS CodeStar Connections is a managed service that allows you to automate the creation, configuration, and management of connections to other AWS services, third-party tools, or self-hosted tools. It provides a convenient way to integrate and interact with your project's external integrations seamlessly.

## Understanding the ResourceNotFoundException
The `ResourceNotFoundException` is an exception class representing the non-existence of a requested resource within the CodeStar Connections service. This exception is thrown when an API request refers to a resource that is either deleted, not available, or does not exist.

Whenever you receive a `ResourceNotFoundException`, it is crucial to handle it gracefully. Let's dive into some possible causes and solutions to help you troubleshoot and resolve this exception.

## Possible Scenarios and Solutions
### Case 1: Missing Connection
The most common scenario for the `ResourceNotFoundException` is when the requested connection does not exist. This can happen when you mistakenly provide an invalid or non-existent connection ARN (Amazon Resource Name).

**Solution**: Before performing any operations on a connection, ensure that it exists by listing all available connections with the [ListConnections](https://docs.aws.amazon.com/codestar-connections/latest/APIReference/API_ListConnections.html) API. Once you have the list, you can verify the presence of the connection with the specific ARN you intend to use.

```java
// Example Java code to list connections
import com.amazonaws.services.codestarconnections.AWSCodeStarconnections;
import com.amazonaws.services.codestarconnections.model.Connection;
import com.amazonaws.services.codestarconnections.model.ListConnectionsRequest;
import com.amazonaws.services.codestarconnections.model.ListConnectionsResult;

AWSCodeStarconnections client = AWSCodeStarconnectionsClientBuilder.defaultClient();
ListConnectionsResult result = client.listConnections(new ListConnectionsRequest());
List<Connection> connections = result.getConnections();

// Iterate through the connections and check if the desired connection exists.
```

### Case 2: Invalid ARN Format
Another reason for the `ResourceNotFoundException` is when you provide an invalid ARN format while making API requests.

**Solution**: Ensure that you are providing a correctly formatted ARN as per AWS guidelines. Refer to the [Amazon Resource Names (ARNs) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) documentation for the correct syntax of ARNs.

```python
import boto3

client = boto3.client('codestar-connections')
response = client.create_connection(
    ConnectionName='MyConnection',
    ProviderType='GitHub',
    ...
)

print(response['ConnectionArn'])
```

### Case 3: Timing Constraints
Sometimes, the `ResourceNotFoundException` may occur due to a timing mismatch. It could be a result of performing operations on a resource that is being concurrently created, updated, or deleted.

**Solution**: Implement exponential backoff and retry mechanisms to handle such transient conditions. Retry the failed API request after a short delay, gradually increasing the interval between retries until the operation succeeds or a maximum retry limit is reached. This approach helps to mitigate timing conflicts and ensures the smooth execution of your code.

```javascript
// Example JavaScript code using AWS SDK for JavaScript
const AWS = require('aws-sdk');
const codestarconnections = new AWS.CodeStarconnections();

function describeConnection(connectionArn, retriesLeft = 3) {
  codestarconnections.describeConnection({ ConnectionArn: connectionArn }, (err, data) => {
    if (err) {
      if (err.code === 'ResourceNotFoundException' && retriesLeft > 0) {
        // Retry after a short delay
        setTimeout(() => describeConnection(connectionArn, retriesLeft - 1), 1000);
      } else {
        console.log(err, err.stack);
      }
    } else {
      console.log(data);
    }
  });
}
```

### Case 4: Permission Issues
The `ResourceNotFoundException` may also occur if the IAM user or role used for your API request does not have the necessary permissions to access the CodeStar Connections service or the specific connection resource.

**Solution**: Ensure that the associated IAM user or role has appropriate permissions to interact with the CodeStar Connections APIs. Refer to the [AWS Identity and Access Management (IAM) documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) to set up the required permissions correctly.

## Conclusion
In this article, we explored the `ResourceNotFoundException` in AWS CodeStar Connections and examined various scenarios that can cause this exception. By understanding the possible causes and implementing the proposed solutions, you can effectively troubleshoot and resolve this error in your application.

Always remember to handle exceptions gracefully and follow best practices when working with AWS CodeStar Connections. By doing so, you will be able to integrate and manage your project's external resources effortlessly.

Keep exploring the AWS CodeStar Connections documentation for in-depth knowledge, and don't hesitate to reach out to the AWS Support team if you still face any challenges.

Happy coding!

**References:**
- [AWS CodeStar Connections Documentation](https://docs.aws.amazon.com/codestar-connections/latest/APIReference/Welcome.html)
- [Working with AWS CodeStar Connections](https://docs.aws.amazon.com/codestar-connections/latest/APIReference/Welcome.html)
- [Amazon Resource Names (ARNs) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)
- [AWS Identity and Access Management (IAM) Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)