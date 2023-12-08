---
title: "EventDataStoreTerminationProtectedException in AWS CloudTrail"
date: 2024-01-06 09:00:00 -0000
categories: [AWS, AWS CloudTrail]
tags: [aws, cloudtrail, com.amazonaws.services.cloudtrail.model]
mermaid: true
toc: true
---


## Introduction

AWS CloudTrail is a service that enables auditing and monitoring of activity within an AWS account. It provides a detailed history of events, such as API calls and resource changes, allowing you to track user activity and troubleshoot operational issues. In this article, we will discuss a specific exception called `EventDataStoreTerminationProtectedException` that you may encounter while using AWS CloudTrail.

## What is EventDataStoreTerminationProtectedException?

`EventDataStoreTerminationProtectedException` is an exception that occurs when attempting to delete the event data store associated with an AWS CloudTrail trail. This exception is thrown by the `deleteTrail` method of the `com.amazonaws.services.cloudtrail.model.AWSCloudTrailClient` class. It indicates that the event data store cannot be deleted because termination protection is enabled.

## Understanding Termination Protection

Termination protection is a feature in AWS CloudTrail that helps prevent accidental or unauthorized deletion of your event data store. When enabled, it prevents the deletion of the event data store associated with a trail. This ensures that your data is protected and cannot be deleted without deliberate action.

## Handling EventDataStoreTerminationProtectedException

When you encounter an `EventDataStoreTerminationProtectedException`, it means that termination protection is enabled for the event data store associated with the specified trail. To resolve this issue and delete the event data store, you must first disable the termination protection.

To disable termination protection, you can use the AWS Management Console, AWS CLI, or AWS SDKs. Here's an example using the AWS CLI:

```shell
aws cloudtrail update-trail --name my-trail --no-enable-log-file-validation --no-is-multi-region-trail --apply-immediately --no-cloud-watch-logs-log-encryption-enabled --no-s3-bucket-policy --no-include-global-service-events --no-is-organization-trail --no-kms-key-id --no-s3-key-prefix --no-sns-topic-name --no-trail-arn --no-include-management-events --no-s3-new-prefix
```

In this example, we are updating the trail named `my-trail` with several options set to `no`. This effectively disables the termination protection for the event data store.

Once termination protection is disabled, you can retry deleting the event data store using the `deleteTrail` method. Remember to re-enable termination protection after you have completed the necessary operations.

## Conclusion

In this article, we discussed the `EventDataStoreTerminationProtectedException` that can occur when attempting to delete the event data store associated with an AWS CloudTrail trail. We explained how termination protection works and how to handle this exception by disabling termination protection before attempting to delete the data store.

To learn more about AWS CloudTrail and its API, refer to the official AWS documentation:

- [AWS CloudTrail Documentation](https://docs.aws.amazon.com/cloudtrail/index.html)

Be sure to always check for the latest guidelines and updates from AWS to ensure you follow the best practices when working with AWS CloudTrail.

Happy auditing and monitoring with AWS CloudTrail!