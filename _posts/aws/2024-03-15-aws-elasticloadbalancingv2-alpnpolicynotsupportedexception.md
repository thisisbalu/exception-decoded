---
title: "ALPNPolicyNotSupportedException in AWS Elastic Load Balancing V2"
date: 2024-03-15 09:00:00 -0000
categories: [AWS, AWS Elastic Load Balancing V2]
tags: [aws, elasticloadbalancingv2, com.amazonaws.services.elasticloadbalancingv2.model]
mermaid: true
toc: true
---


## Introduction

In the world of AWS Elastic Load Balancing V2, there are various exceptions that developers may encounter. One such exception is the ALPNPolicyNotSupportedException. This article dives deep into understanding this exception, its causes, and possible solutions. We'll explore the various use cases where this exception may occur and how to handle it.

## What is ALPN?

Before we dive into the exception, let's understand what ALPN is. ALPN stands for Application-Layer Protocol Negotiation. It's an extension to the TLS protocol that allows clients and servers to negotiate which protocol to use within the TLS handshake. This negotiation is essential for enabling multiple protocols to run over a secure connection.

## ALPNPolicyNotSupportedException

In AWS Elastic Load Balancing V2, the ALPNPolicyNotSupportedException is thrown when the specified ALPN policy is not supported by the target group. This exception occurs when the target group receives a request with an ALPN policy that is not configured or enabled.

The ALPNPolicyNotSupportedException belongs to the com.amazonaws.services.elasticloadbalancingv2.model package and is an instance of the class com.amazonaws.services.elasticloadbalancingv2.model.ALPNPolicyNotSupportedException.

### Causes of ALPNPolicyNotSupportedException

The ALPNPolicyNotSupportedException can be caused by the following:

1. **Invalid ALPN Policy:** If the target group's configured ALPN policy does not match the ALPN policy received in the request, this exception is thrown.
2. **Missing ALPN Policy Configuration:** If the target group does not have any ALPN policies configured or enabled, this exception may occur.

### Handling ALPNPolicyNotSupportedException

To handle the ALPNPolicyNotSupportedException, you can follow the below steps:

1. **Check ALPN Policy Configuration:** Verify that the ALPN policy specified in the request matches the ones configured and enabled in the target group. You can use the `describeTargetGroupAttributes` API to fetch the current ALPN policy configuration of the target group.

   ```java
   DescribeTargetGroupAttributesRequest describeTargetGroupAttributesRequest =
       new DescribeTargetGroupAttributesRequest()
           .withTargetGroupArn("arn:aws:elasticloadbalancing:us-west-2:123456789012:targetgroup/my-target-group/1234567890123456");

   DescribeTargetGroupAttributesResult describeTargetGroupAttributesResult =
       elasticLoadBalancingClient.describeTargetGroupAttributes(describeTargetGroupAttributesRequest);

   List<TargetGroupAttribute> targetGroupAttributes =
       describeTargetGroupAttributesResult.getAttributes();

   for (TargetGroupAttribute attribute : targetGroupAttributes) {
       if (attribute.getKey().equals("alpn-policy")) {
           String currentALPNPolicy = attribute.getValue();
           // Compare the currentALPNPolicy with the expected ALPN policy in the request
       }
   }
   ```

2. **Update ALPN Policy Configuration:** If the specified ALPN policy is not configured or enabled, you need to update the target group's ALPN policies using the `modifyTargetGroupAttributes` API.

   ```java
   ModifyTargetGroupAttributesRequest modifyTargetGroupAttributesRequest =
       new ModifyTargetGroupAttributesRequest()
           .withTargetGroupArn("arn:aws:elasticloadbalancing:us-west-2:123456789012:targetgroup/my-target-group/1234567890123456")
           .withAttributes(new TargetGroupAttribute().withKey("alpn-policy").withValue("HTTP2"));

   elasticLoadBalancingClient.modifyTargetGroupAttributes(modifyTargetGroupAttributesRequest);
   ```

### Conclusion

The ALPNPolicyNotSupportedException is an exception that can occur in AWS Elastic Load Balancing V2 when the ALPN policy specified in the request is not configured or enabled in the target group. In this article, we explored the causes of this exception and how to handle it. Remember to always verify the ALPN policy configuration and update it if necessary to ensure the smooth functioning of your target group.

To find more information about the ALPNPolicyNotSupportedException and related concepts, refer to the following links:

- [ALPN (Application-Layer Protocol Negotiation)](https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation)
- [ALPNPolicyNotSupportedException - AWS SDK for Java Documentation](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/elasticloadbalancingv2/model/ALPNPolicyNotSupportedException.html)
- [AWS Elastic Load Balancing V2 Documentation](https://docs.aws.amazon.com/elasticloadbalancing/)

Hope you found this article informative and useful!