---
title: "Understanding MaxDocumentSizeExceededException in AWS Simple Systems Management SSM
Original large script example
Simulated actions
Optimized script
Simulated actions"
date: 2024-12-11 09:00:00 -0000
categories: [AWS, AWS Simple Systems Management (SSM)]
tags: [aws, simplesystemsmanagement, com.amazonaws.services.simplesystemsmanagement.model]
mermaid: true
toc: true
---


AWS Simple Systems Management (SSM) streamlines your AWS resource management, allowing for efficient execution of commands across multiple instances. However, while using this vital service, you may encounter the `MaxDocumentSizeExceededException`. In this article, we'll delve deep into what this exception means, when it arises, and how to effectively manage and avoid it.

## What is MaxDocumentSizeExceededException?

The `MaxDocumentSizeExceededException` is a specific exception thrown by the AWS SDK when the size of a document being sent to AWS SSM exceeds the maximum allowed size. This document typically contains commands or scripts to be executed on your instances. AWS enforces limits on the size of these documents to efficiently manage resources and maintain performance.

### Maximum Document Size

According to AWS documentation, the maximum size for a command document (in bytes) is currently set to 64 KB. If you try to send a command document larger than this size, you will encounter the `MaxDocumentSizeExceededException`.

## Common Scenarios Leading to MaxDocumentSizeExceededException

Several scenarios might lead to this exception:

1. **Large Command Payload**: If your command script is lengthy, it may exceed the maximum size.
2. **Complex Script**: Composite scripts involving multiple commands could significantly increase the document size.
3. **Excessive Parameters**: Sometimes, an excessive number of parameters can inflate the document size.

## How to Handle MaxDocumentSizeExceededException

Handling the `MaxDocumentSizeExceededException` depends on understanding your scripts and optimizing them to fit within the size constraints imposed by AWS. Below are strategies you can adopt:

### Strategy 1: Optimize Your Scripts

Start by reviewing your command scripts for any potential optimizations. Remove unnecessary commands, comments, or redundant logic.

```bash
echo "Starting process"
sleep 1
echo "Middle process"
sleep 1
echo "Ending process"
```

An optimized script could be:

```bash
echo "Starting process"
sleep 2
```

### Strategy 2: Use S3 for Long Scripts

If your script exceeds limits even after optimization, consider storing it in an S3 bucket and retrieving it during execution.

```java
import com.amazonaws.services.simplesystemsmanagement.AmazonSSM;
import com.amazonaws.services.simplesystemsmanagement.AmazonSSMClientBuilder;
import com.amazonaws.services.simplesystemsmanagement.model.SendCommandRequest;
import com.amazonaws.services.simplesystemsmanagement.model.Command;

public class RunCommandFromS3 {
    public static void main(String[] args) {
        AmazonSSM ssm = AmazonSSMClientBuilder.defaultClient();
        
        // S3 path for your script
        String s3ScriptPath = "s3://your-bucket/your-script.sh";

        SendCommandRequest sendCommandRequest = new SendCommandRequest()
                .withDocumentName("AWS-RunShellScript")
                .withParameters(Collections.singletonMap("commands", Arrays.asList("aws s3 cp " + s3ScriptPath + " . && chmod +x your-script.sh && ./your-script.sh")));

        String commandId = ssm.sendCommand(sendCommandRequest).getCommand().getCommandId();
        System.out.println("Command sent with ID: " + commandId);
    }
}
```

### Strategy 3: Break Down the Command

If your task can be broken down into smaller, separate commands, you can execute them in sequence. This approach can help you stay under the document size limit.

```java
List<String> commands = Arrays.asList(
    "echo 'First part of the process'",
    "echo 'Second part of the process'",
    "echo 'Finalizing process'"
);

SendCommandRequest sendCommandRequest = new SendCommandRequest()
        .withDocumentName("AWS-RunShellScript")
        .withParameters(Collections.singletonMap("commands", commands));
```

## Conclusion

The `MaxDocumentSizeExceededException` is a common hurdle when working with AWS SSM. However, with a sound understanding of your command documents and effective strategies for optimization, you can easily manage and circumvent this issue. 

Efficient scripting and integrating with AWS services like S3 can help you get the most out of SSM without running into size limit obstacles.

## References

- [MaxDocumentSizeExceededException Documentation](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_MaxDocumentSizeExceededException.html)
- [AWS SSM Command Documents](https://docs.aws.amazon.com/systems-manager/latest/userguide/documents-structure.html)
- [Using S3 with AWS SSM](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-automation-invoke-s3.html)