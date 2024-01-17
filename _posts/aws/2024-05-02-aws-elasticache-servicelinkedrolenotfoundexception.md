---
title: "ServiceLinkedRoleNotFoundException in AWS ElastiCache"
date: 2024-05-02 09:00:00 -0000
categories: [AWS, AWS ElastiCache]
tags: [aws, elasticache, com.amazonaws.services.elasticache.model]
mermaid: true
toc: true
---


Have you ever encountered the `ServiceLinkedRoleNotFoundException` while working with AWS ElastiCache? If so, you're not alone. In this article, we will explore the reasons behind this exception and how to handle it effectively.

## What is ServiceLinkedRoleNotFoundException?

The `ServiceLinkedRoleNotFoundException` is an AWS ElastiCache-specific exception that occurs when the service-linked role required by ElastiCache is not found in your AWS account. ElastiCache uses service-linked roles to securely grant permissions to other AWS services it integrates with.

## Causes of ServiceLinkedRoleNotFoundException

There are a few reasons why you may encounter the `ServiceLinkedRoleNotFoundException`:

### 1. Role Not Created

The most common reason for this exception is that the service-linked role required by ElastiCache is not created in your AWS account. This role is automatically created by AWS when you use the ElastiCache service for the first time. However, in some cases, the role may have been deleted accidentally or by a user without appropriate permissions.

### 2. Role Not Associated

Another possible cause is that the role exists but is not associated with the ElastiCache service. This can happen if the role is manually disassociated from ElastiCache or if it is associated with a different AWS resource.

### 3. Permissions Revoked

The service-linked role requires certain permissions to function correctly. If these permissions are revoked or modified, ElastiCache may not be able to find or use the required role, leading to the `ServiceLinkedRoleNotFoundException`.

## How to Resolve the ServiceLinkedRoleNotFoundException?

To resolve the `ServiceLinkedRoleNotFoundException`, follow these steps:

### 1. Create the Service-Linked Role

If the service-linked role does not exist in your AWS account, you can create it manually using the AWS Management Console, AWS CLI, or AWS SDKs. Here's an example using AWS CLI:

```shell
$ aws iam create-service-linked-role --aws-service-name elasticache.amazonaws.com
```

After executing the above command, AWS will create the necessary role and associate it with the ElastiCache service.

### 2. Check Role Association

Make sure that the service-linked role is associated with the ElastiCache service. You can check this using the AWS Management Console or the `list-roles` command in AWS CLI:

```shell
$ aws iam list-roles --query "Roles[?AssumeRolePolicyDocument.Statement[0].Principal.Service=='elasticache.amazonaws.com'] | [0]"
```

If the command returns the required service-linked role, it means that the role is associated correctly.

### 3. Verify Role Permissions

Ensure that the service-linked role has the necessary permissions. You can find the required permissions in the [AWS ElastiCache User Guide](https://docs.aws.amazon.com/elasticache/latest/red-ug/elasticache-service-linked-roles.html).

If the required permissions are missing or modified, you can use the [AWS Management Console](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify-service-linked-role.html) or the AWS CLI to update the role's policy document.

## Conclusion

The `ServiceLinkedRoleNotFoundException` is a common exception that can occur when using AWS ElastiCache. In this article, we discussed the causes behind this exception and provided steps to resolve it. By following these steps, you can ensure that the necessary service-linked role is created, associated, and has the required permissions, allowing ElastiCache to function correctly.

If you want to learn more about service-linked roles and their usage in other AWS services, check out the [AWS Service-Linked Roles documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html#service-linked-role-supported-product-elasticache).

Now that you are equipped with the knowledge to handle the `ServiceLinkedRoleNotFoundException`, you can confidently work with AWS ElastiCache without any interruption of service. Happy caching!