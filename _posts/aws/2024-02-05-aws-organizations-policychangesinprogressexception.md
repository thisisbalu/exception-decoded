---
title: "**PolicyChangesInProgressException in AWS Organizations**"
date: 2024-02-05 09:00:00 -0000
categories: [AWS, AWS Organizations]
tags: [aws, organizations, com.amazonaws.services.organizations.model]
mermaid: true
toc: true
---


## Introduction
AWS Organizations is a service that helps you centrally manage and govern your AWS accounts. It allows you to organize and maintain multiple accounts under a single umbrella. To make changes to the policies defined for your organization, AWS Organizations provides a comprehensive set of APIs. One of the exceptions you may encounter when working with these APIs is the `PolicyChangesInProgressException`. In this article, we will dive deep into this exception, understand its implications, causes, and explore ways to handle it effectively.

## Understanding `PolicyChangesInProgressException`
The `PolicyChangesInProgressException` is thrown by the `com.amazonaws.services.organizations.model` in AWS Organizations. It indicates that there are policy changes currently in progress, and therefore, the requested API operation cannot be completed until those changes are completed or canceled.

## Causes of `PolicyChangesInProgressException`
Several factors can lead to the occurrence of the `PolicyChangesInProgressException`. Let's explore some of the common causes:

### 1. Existing Policy Change Requests
If there are any pending policy change requests, such as creating, updating, or deleting a policy, the `PolicyChangesInProgressException` will be thrown. You may encounter this exception when trying to initiate any new policy change requests while an existing one is still pending.

### 2. Stacked Requests
Sometimes, multiple policy change requests can be queued up simultaneously. In such cases, when you attempt to perform a new policy change request while previous requests are still being processed, the `PolicyChangesInProgressException` may be thrown.

### 3. Time-limited Constraints
AWS Organizations applies time-limited constraints to ensure consistency and avoid race conditions in policy changes. If you try to initiate multiple requests within a short timespan, the `PolicyChangesInProgressException` might be thrown.

## Handling `PolicyChangesInProgressException`
To handle the `PolicyChangesInProgressException` more gracefully, you can utilize the information provided by the exception and adopt a retry strategy. Here's an example code snippet demonstrating how you can handle this exception in Java:

```java
try {
    // Make AWS Organizations API request
} catch (PolicyChangesInProgressException ex) {
    // Display a message indicating policy changes are in progress
    System.out.println("Policy changes are currently in progress. Please wait and try again.");
}
```

## Retry Strategy for `PolicyChangesInProgressException`
To effectively handle the `PolicyChangesInProgressException`, a retry strategy can be implemented. Here's an example of a retry function in Python, using the `botocore` library:

```python
import botocore

def retry_on_policy_changes(func):
    def inner(*args, **kwargs):
        retry_count = 0
        while True:
            try:
                return func(*args, **kwargs)
            except botocore.exceptions.PolicyChangesInProgressException:
                print("Policy changes are in progress. Retrying...")
                retry_count += 1
                if retry_count == 5:
                    raise
    return inner
```

In the above code, `func` represents the actual AWS Organizations API call that you wish to make. The `retry_on_policy_changes` function catches the `PolicyChangesInProgressException`, displays a message, and retries the API call. It retries up to 5 times before rethrowing the exception.

## Conclusion
In this article, we discussed the `PolicyChangesInProgressException` in AWS Organizations. We looked at its causes and how to handle it gracefully. By leveraging the information provided by this exception and implementing a retry strategy, you can effectively navigate and manage policy changes in your AWS organization.

Please remember that handling this exception in your code is crucial for ensuring a smooth experience when working with AWS Organizations APIs. By adopting best practices and following AWS's recommendations, you can maintain control and manage your organization's policies efficiently.

To learn more about AWS Organizations and the exceptions it may throw, refer to the official AWS documentation: [AWS Organizations Documentation][1].

Happy coding, and keep building amazing things with AWS!

## References
- [AWS Organizations Documentation][1]

[1]: https://docs.aws.amazon.com/organizations/index.html