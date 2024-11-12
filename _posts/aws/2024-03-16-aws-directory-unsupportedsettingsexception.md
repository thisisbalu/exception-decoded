---
title: "**The UnsupportedSettingsException in AWS Directory Service: A Comprehensive Guide**"
date: 2024-03-16 09:00:00 -0000
categories: [AWS, AWS Directory Service]
tags: [aws, directory, com.amazonaws.services.directory.model]
mermaid: true
toc: true
---


When working with AWS Directory Service, you may encounter various exceptions that help pinpoint and handle any issues with your directory configuration. One such common exception is the `UnsupportedSettingsException`. In this article, we will delve deep into this exception, understand its causes, and explore potential solutions.

## Understanding the UnsupportedSettingsException
The `com.amazonaws.services.directory.model.UnsupportedSettingsException` is thrown when attempting to create or modify a directory with unsupported configuration settings. This exception helps enforce valid inputs and maintain the integrity of your AWS Directory Service.

Let's take a closer look at the structure and key properties of this exception in Java:
```java
public class UnsupportedSettingsException extends com.amazonaws.AmazonServiceException {
   private static final long serialVersionUID = 1L;
   
   private java.util.List<String> unsupportedSettings;

   /**
   * Returns the unsupported configuration settings associated with the exception.
   *
   * @return List of unsupported configuration settings
   */
   public java.util.List<String> getUnsupportedSettings() {
      return unsupportedSettings;
   }
}
```

## Causes of the UnsupportedSettingsException
The `UnsupportedSettingsException` can occur due to several reasons. Here are a few scenarios where this exception might be thrown:

### 1. Unsupported Directory Type
AWS Directory Service supports multiple directory types, such as Simple AD, Microsoft AD, and AD Connector. Each directory type has its list of supported configuration settings. If you attempt to configure unsupported settings for a specific directory type, the `UnsupportedSettingsException` will be raised. 

For example, when creating a Microsoft AD directory, the following settings may be considered unsupported:
```java
CreateDirectoryRequest request = new CreateDirectoryRequest()
   .withName("myDirectory")
   .withDirectoryType(DirectoryType.MicrosoftAD)
   .withShortName("mysubdomain")
   .withPassword("myPassword")
   .withUnsupportedSettings("UnsupportedSetting1", "UnsupportedSetting2");
```

### 2. Invalid Directory Size
AWS Directory Service offers directory size options, such as Small, Large, and ExtraLarge. Each directory size has specific resource allocations, including CPU, memory, and storage. If you specify an unsupported or invalid directory size, the `UnsupportedSettingsException` will be thrown.

For instance, the following code will trigger the exception due to the use of an invalid directory size:
```java
CreateDirectoryRequest request = new CreateDirectoryRequest()
   .withName("myDirectory")
   .withDirectoryType(DirectoryType.SimpleAD)
   .withPassword("myPassword")
   .withSize("UnsupportedSize");
```

### 3. Unsupported Advanced Security Mode
AWS Directory Service offers an advanced security mode for Microsoft AD directories. When enabling advanced security, you can choose either `ENABLED` or `DISABLED`. If you attempt to set an unsupported value for the advanced security mode, the `UnsupportedSettingsException` will be raised.

Here's an example that demonstrates an unsupported advanced security mode:
```java
UpdateConditionalForwarderRequest request = new UpdateConditionalForwarderRequest()
   .withDirectoryId("myDirectoryId")
   .withRemoteDomainName("example.com")
   .withDnsIpAddrs("192.0.2.1", "192.0.2.2")
   .withunSupportedAdvancedSecurityMode("AnyValue");
```

## Handling the UnsupportedSettingsException
To handle the `UnsupportedSettingsException` gracefully, you can make use of exception handling techniques. When catching this exception, you can log the specific error details, display user-friendly messages, or even revert to default settings.

Here's an example of catching and handling the `UnsupportedSettingsException` in Java:
```java
try {
   CreateDirectoryResult result = directoryService.createDirectory(request);
   // Directory creation successful
} catch (UnsupportedSettingsException ex) {
   List<String> unsupportedSettings = ex.getUnsupportedSettings();
   // Log the unsupported settings or display a user-friendly error message
}
```

It is crucial to understand the specific unsupported settings associated with the exception to take appropriate action.

## Conclusion
The `UnsupportedSettingsException` in AWS Directory Service helps maintain consistency and safeguards the configuration of your directories. By throwing this exception, AWS ensures that only supported settings and valid inputs are used.

In this article, we explored the `UnsupportedSettingsException`, its causes, and possible handling approaches. Remember to check the AWS Directory Service documentation for a comprehensive list of supported settings and configurations.

Stay tuned for more articles exploring exception handling and best practices in AWS Directory Service!

**References:**
- [AWS Directory Service Documentation](https://docs.aws.amazon.com/directoryservice/latest/api/Welcome.html)
- [CreateDirectoryRequest - AWS SDK for Java Documentation](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/index.html?com/amazonaws/services/directory/model/CreateDirectoryRequest.html)
- [UpdateConditionalForwarderRequest - AWS SDK for Java Documentation](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/index.html?com/amazonaws/services/directory/model/UpdateConditionalForwarderRequest.html)