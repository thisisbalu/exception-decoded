---
title: "NotFoundException of com.amazonaws.services.transcribe.model in AWS Transcribe"
date: 2024-01-30 09:00:00 -0000
categories: [AWS, AWS Transcribe]
tags: [aws, transcribe, com.amazonaws.services.transcribe.model]
mermaid: true
toc: true
---


## Introduction
In the world of transcription services, AWS Transcribe has emerged as a powerful tool that converts speech to text with remarkable accuracy. However, like any complex technology, it is not without its quirks. One such quirk is the `NotFoundException` that developers might encounter when using the `com.amazonaws.services.transcribe.model` library. In this article, we will explore what this exception means, how to handle it, and share some tips to avoid it altogether.

## Understanding the NotFoundException
The `NotFoundException` is an exception that is thrown when a specific resource cannot be found. In the context of AWS Transcribe, it usually occurs when attempting to retrieve information about a job that does not exist or has been previously deleted. The exception class `com.amazonaws.services.transcribe.model.NotFoundException` is part of the official AWS SDK for Java.

Let's take a look at an example of how this exception can be encountered:

```java
import com.amazonaws.services.transcribe.AmazonTranscribe;
import com.amazonaws.services.transcribe.AmazonTranscribeClientBuilder;
import com.amazonaws.services.transcribe.model.GetTranscriptionJobRequest;
import com.amazonaws.services.transcribe.model.GetTranscriptionJobResult;
import com.amazonaws.services.transcribe.model.NotFoundException;

public class TranscriptionService {
    private final AmazonTranscribe transcribeClient;

    public TranscriptionService() {
        // Create a new instance of the Transcribe client
        this.transcribeClient = AmazonTranscribeClientBuilder.defaultClient();
    }

    public void checkTranscriptionStatus(String jobId) {
        GetTranscriptionJobRequest request = new GetTranscriptionJobRequest()
            .withTranscriptionJobName(jobId);

        try {
            GetTranscriptionJobResult result = transcribeClient.getTranscriptionJob(request);
            System.out.println("Transcription status: " + result.getTranscriptionJob().getTranscriptionJobStatus());
        } catch (NotFoundException e) {
            System.out.println("Transcription job not found");
        } catch (Exception e) {
            System.out.println("An error occurred: " + e.getMessage());
        }
    }
}
```

In this example, we have a `TranscriptionService` class that utilizes the AWS Transcribe client to check the status of a transcription job. If the job is not found, a `NotFoundException` is caught and a corresponding message is printed.

## Handling the NotFoundException
When encountering a `NotFoundException`, it's important to handle it gracefully to prevent unexpected behavior or crashes in your application. In the example above, we catch the exception and display an informative message to the user. You may choose to handle it differently based on your application's requirements.

## Tips to Avoid the NotFoundException
While handling exceptions is essential, it's always better to avoid them when possible. Here are a few tips to help prevent the `NotFoundException` in your code:

### 1. Check if the job exists
Before attempting to retrieve information about a specific transcription job, ensure that it actually exists. You can do this by querying the list of available jobs and checking if the desired job ID is present. For example:

```java
import com.amazonaws.services.transcribe.model.ListTranscriptionJobsRequest;
import com.amazonaws.services.transcribe.model.ListTranscriptionJobsResult;

public boolean isJobExists(String jobId) {
    ListTranscriptionJobsRequest request = new ListTranscriptionJobsRequest()
            .withStatus(JobStatus.COMPLETED);

    ListTranscriptionJobsResult result = transcribeClient.listTranscriptionJobs(request);
    List<TranscriptionJobSummary> jobs = result.getTranscriptionJobSummaries();

    return jobs.stream().anyMatch(job -> job.getTranscriptionJobName().equals(jobId));
}
```

By checking the list of completed transcription jobs, you can determine if the job you are looking for exists before attempting to retrieve its details.

### 2. Retry with exponential backoff
In some scenarios, the `NotFoundException` can occur due to temporary network issues or delays in updating the job status. To handle such cases, it's recommended to implement a retry mechanism with exponential backoff. This approach allows your application to automatically retry the request after a certain delay, gradually increasing the wait time in case of consecutive failures.

The AWS SDK provides utilities for implementing this retry mechanism, such as the `RetryUtils` class. Here's an example of how you can leverage it:

```java
import com.amazonaws.services.transcribe.model.TranscriptionJob;
import com.amazonaws.retry.RetryUtils;

public TranscriptionJob waitForJobCompletion(String jobId) {
    GetTranscriptionJobRequest request = new GetTranscriptionJobRequest()
        .withTranscriptionJobName(jobId);

    RetryUtils.RetryableTask<GetTranscriptionJobResult> task = context -> transcribeClient.getTranscriptionJob(request);

    try {
        RetryUtils.executeWithRetry(task);
        GetTranscriptionJobResult result = task.getResult();
        return result.getTranscriptionJob();
    } catch (RetryUtils.RetryableTaskFailedException e) {
        System.out.println("Job retrieval failed despite retries: " + e.getMessage());
        return null;
    } 
}
```

In this example, the `RetryUtils.executeWithRetry()` method wraps the retrieval of the job and handles the retries internally if the `NotFoundException` occurs.

## Conclusion
The `NotFoundException` can be an inconvenience when working with AWS Transcribe, but with the right understanding and practices, it can be easily managed. By handling the exception gracefully, checking if the job exists before retrieval, and implementing retry mechanisms when necessary, you can minimize the impact of this exception on your application.

If you want to learn more about AWS Transcribe and its integration with other services, refer to the official documentation:

- [AWS Transcribe Developer Guide](https://docs.aws.amazon.com/transcribe/latest/dg/welcome.html)
- [AWS SDK for Java](https://docs.aws.amazon.com/sdk-for-java/index.html)
- [AWS SDK for Java - AWS Transcribe API Reference](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/transcribe/AmazonTranscribe.html)

With these best practices and knowledge in your repertoire, you can confidently tackle the NotFoundException in AWS Transcribe and deliver exceptional transcription services to your users.