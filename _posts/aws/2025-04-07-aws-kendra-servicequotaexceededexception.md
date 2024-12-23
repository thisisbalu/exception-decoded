---
title: "Understanding ServiceQuotaExceededException in AWS Kendra"
date: 2025-04-07 09:00:00 -0000
categories: [AWS, AWS Kendra]
tags: [aws, kendra, com.amazonaws.services.kendra.model]
mermaid: true
toc: true
---


AWS Kendra is a powerful, intelligent search service powered by machine learning that allows organizations to gather insights from their data. However, while working with Kendra, developers may encounter various exceptions that can disrupt their applications. One such exception is the `ServiceQuotaExceededException`. This article provides a detailed exploration of this exception, its causes, and how to handle it effectively within your AWS Kendra implementations.

## What is ServiceQuotaExceededException?

The `ServiceQuotaExceededException` is an exception that occurs when an application reaches the service limit imposed by AWS on the Kendra service. Each service in AWS comes with default quotas and limits, which can influence how an application interacts with the service. When these limits are exceeded, AWS Kendra throws this specific exception.

### Common Causes

1. **Exceeded Document Count**: Kendra has a limit on the number of documents that can be indexed within a specific data source. Exceeding this limit triggers the exception.
2. **Request Rate Limits**: AWS imposes limits on how many requests can be made to a service within a specific timeframe. Sending too many requests can lead to this error.
3. **Data Source Limitations**: Adding data sources beyond the allowed limit for Kendra can result in the `ServiceQuotaExceededException`.
4. **Search Queries Limitation**: Running numerous search queries simultaneously can lead to an exceeded quota.

### How to Handle ServiceQuotaExceededException

To efficiently handle the `ServiceQuotaExceededException`, you can implement various strategies in your application to provide a better user experience while adhering to AWS Kendra's limits.

#### 1. Catching the Exception

In your Java application utilizing the AWS SDK for Java, you can catch this specific exception and implement fallback logic. Here’s an example:

```java
import com.amazonaws.services.kendra.model.ServiceQuotaExceededException;
import com.amazonaws.services.kendra.AWSKendra;
import com.amazonaws.services.kendra.AWSKendraClientBuilder;

public class KendraExample {

    private static AWSKendra kendraClient = AWSKendraClientBuilder.defaultClient();

    public void performSearch() {
        try {
            kendraClient.query(/* query parameters */);
        } catch (ServiceQuotaExceededException e) {
            System.out.println("AWS Kendra quota exceeded: " + e.getMessage());
            handleQuotaExceeded();
        }
    }

    private void handleQuotaExceeded() {
        // Implement retry logic or notify users here
        System.out.println("Implementing fallback logic or notifying users.");
    }
}
```

#### 2. Implementing Exponential Backoff

When handling rate limit exceptions, it’s a best practice to implement an exponential backoff strategy. This method gradually increases the wait time between each retry after a failure, helping ease the burden on the service.

```java
private void retrySearchWithBackoff(int retries) {
    long waitTime = (long) Math.pow(2, retries) * 100; // Exponential backoff

    try {
        Thread.sleep(waitTime);
        performSearch(); // Retry search
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }   
}
```

#### 3. Monitoring Quotas

AWS provides a mechanism to monitor service quotas via the AWS Management Console or AWS CLI. Utilizing CloudWatch metrics, you can keep track of your Kendra usage and proactively manage your resources.

To view the quotas from the console:

1. Log in to the AWS Management Console.
2. Navigate to the "Service Quotas" section.
3. Select "AWS Kendra" from the list of services to view detailed limits.

#### 4. Requesting Quota Increases

If your application consistently hits the service quota limits, consider requesting a limit increase. This can be done through the AWS Support Center. Here’s how to request an increase:

1. Go to the AWS Support Center.
2. Click on "Create case."
3. Choose "Service limit increase."
4. Fill in the necessary details about your request.

### Best Practices to Avoid ServiceQuotaExceededException

- **Implement Rate Limiting**: Control the number of requests to Kendra within a specified time frame.
- **Batch Processing**: When indexing documents, batch them in chunks instead of sending them all at once.
- **Optimize Search Queries**: Refine your search queries to ensure they are efficient and do not overwhelm the service.
- **Use AWS SDKs**: AWS SDKs handle many underlying complexities, including retries, which can help reduce the chances of hitting limits.

### Conclusion

The `ServiceQuotaExceededException` in AWS Kendra is an important exception that developers need to be aware of when implementing search functionalities in their applications. By understanding what causes this exception and how to effectively handle it, you can ensure a smoother user experience and maintain the integrity of your applications. Implementing best practices and monitoring your usage can significantly reduce the likelihood of encountering such issues in production environments.

### References

- [AWS Kendra Documentation](https://docs.aws.amazon.com/kendra/latest/dg/what-is-kendra.html)
- [AWS Service Quotas](https://docs.aws.amazon.com/servicequotas/latest/userguide/what-is.html)
- [Exponential Backoff and Retry Strategies](https://aws.amazon.com/blogs/aws/implementing-exponential-backoff-in-your-applications/)
- [AWS SDK for Java Documentation](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/home.html)