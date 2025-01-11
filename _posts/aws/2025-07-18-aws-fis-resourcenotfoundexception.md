---
title: "Understanding ResourceNotFoundException in AWS Fault Injection Simulator"
date: 2025-07-18 09:00:00 -0000
categories: [AWS, AWS Fault Injection Simulator]
tags: [aws, fis, com.amazonaws.services.fis.model]
mermaid: true
toc: true
---


The AWS Fault Injection Simulator (FIS) is a powerful tool designed to improve application resilience by allowing you to simulate various failure conditions. One of the exceptions you might encounter while using FIS is `ResourceNotFoundException`. This article delves into what this exception is, why it occurs, and how to handle it effectively in your AWS applications.

## What is ResourceNotFoundException?

`ResourceNotFoundException` is an exception thrown by the AWS SDK, particularly in the context of the `com.amazonaws.services.fis.model` package. This exception indicates that the specified resource could not be found when a request was made. In the case of the AWS Fault Injection Simulator, this could occur for several reasons, such as trying to access a non-existing FIS experiment or resource.

## Common Scenarios Leading to ResourceNotFoundException

### 1. Experiment Not Found

When trying to start or manage an experiment in AWS FIS, a common cause of `ResourceNotFoundException` is referencing an experiment that does not exist. This can happen if the experiment was deleted or never created.

### 2. Target Resource Issues

If the resources you specify as targets for your fault injection are incorrectly referenced (for example, an EC2 instance or ECS service), you might receive this exception.

### 3. Incorrect Identifiers

Using incorrect identifiers for resources like IAM roles, CloudWatch alarms, or tags associated with the experiment can also lead to this exception.

## How to Handle ResourceNotFoundException

Handling exceptions properly is crucial for maintaining application reliability. Below are strategies and code examples to effectively deal with `ResourceNotFoundException`.

### Example Code: Check If Experiment Exists Before Starting

Before starting an experiment, you can check if it exists. Here is an example in Java using the AWS SDK for Java:

```java
import com.amazonaws.services.fis.AmazonFIS;
import com.amazonaws.services.fis.AmazonFISClientBuilder;
import com.amazonaws.services.fis.model.GetExperimentRequest;
import com.amazonaws.services.fis.model.GetExperimentResult;
import com.amazonaws.services.fis.model.ResourceNotFoundException;

public class FISExperimentChecker {
    private AmazonFIS fisClient = AmazonFISClientBuilder.defaultClient();

    public void checkExperiment(String experimentId) {
        GetExperimentRequest request = new GetExperimentRequest().withId(experimentId);
        
        try {
            GetExperimentResult result = fisClient.getExperiment(request);
            System.out.println("Experiment Details: " + result.getExperiment());
        } catch (ResourceNotFoundException e) {
            System.err.println("Experiment not found: " + e.getMessage());
            // Handle the absence of the experiment accordingly
        }
    }
}
```

### Example Code: Safeguarding Target Resource References

When planning to execute a fault injection on resources, make sure that the reference IDs are correct. Here's how you can handle this in your fault injection setup.

```java
import com.amazonaws.services.fis.AmazonFIS;
import com.amazonaws.services.fis.AmazonFISClientBuilder;
import com.amazonaws.services.fis.model.StartExperimentRequest;
import com.amazonaws.services.fis.model.StartExperimentResult;
import com.amazonaws.services.fis.model.ResourceNotFoundException;

public class FISExperimentRunner {
    private AmazonFIS fisClient = AmazonFISClientBuilder.defaultClient();

    public void startExperiment(String experimentId) {
        StartExperimentRequest request = new StartExperimentRequest().withId(experimentId);
        
        try {
            StartExperimentResult result = fisClient.startExperiment(request);
            System.out.println("Experiment started: " + result.getId());
        } catch (ResourceNotFoundException e) {
            System.err.println("Unable to start experiment, resource not found: " + e.getMessage());
            // Implement fallback or logging logic
        }
    }
}
```

### Example Code: Handling ResourceNotFoundException in a Retry Logic

In some cases, it might make sense to implement retry logic if the resource might become available shortly after a failure. Below is a basic implementation of exponential backoff retry for starting an experiment.

```java
import com.amazonaws.services.fis.AmazonFIS;
import com.amazonaws.services.fis.AmazonFISClientBuilder;
import com.amazonaws.services.fis.model.StartExperimentRequest;
import com.amazonaws.services.fis.model.StartExperimentResult;
import com.amazonaws.services.fis.model.ResourceNotFoundException;

public class FISExperimentWithRetry {
    private AmazonFIS fisClient = AmazonFISClientBuilder.defaultClient();
    private static final int MAX_RETRIES = 3;

    public void startExperimentWithRetry(String experimentId) {
        StartExperimentRequest request = new StartExperimentRequest().withId(experimentId);
        
        for (int i = 0; i < MAX_RETRIES; i++) {
            try {
                StartExperimentResult result = fisClient.startExperiment(request);
                System.out.println("Experiment started: " + result.getId());
                return; // Exit if the experiment starts successfully
            } catch (ResourceNotFoundException e) {
                System.err.println("Resource not found, attempt " + (i + 1) + ": " + e.getMessage());
                // Implement exponential backoff
                try {
                    Thread.sleep((long) Math.pow(2, i) * 1000); // Sleep for 2^i seconds
                } catch (InterruptedException ie) {
                    // Handle interrupt
                }
            }
        }
        System.err.println("Max retries reached. Experiment could not be started.");
    }
}
```

## Prevention Tips

1. **Resource Monitoring**: Regularly monitor and audit your FIS resources to ensure that they are available and properly configured.

2. **Error Handling**: Implement comprehensive error handling in your code to manage various exception scenarios gracefully.

3. **Testing**: Use test cases to simulate various scenarios including non-existent resources to ensure that your error handling is effective.

## Conclusion

`ResourceNotFoundException` can be a common issue when working with the AWS Fault Injection Simulator. By employing the methods and strategies outlined in this article, you can effectively handle this exception, enhancing the resilience of your applications while using FIS. Remember that proactive monitoring and error handling can significantly reduce the likelihood of encountering such issues in a production environment.

## References
- [AWS Fault Injection Simulator Documentation](https://docs.aws.amazon.com/fis/latest/userguide/what-is.html)
- [AWS SDK for Java Documentation](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/home.html)
- [Java Exception Handling](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html)