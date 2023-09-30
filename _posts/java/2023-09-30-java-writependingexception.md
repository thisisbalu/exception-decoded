---
title: "Understanding Java's WritePendingException – An In-Depth Analysis"
date: 2023-09-30 00:41:13 -0000
categories: [Java, java.base]
tags: [java, java-unchecked, java.nio.channels, java-se]
mermaid: true
toc: true
---


When working with `java.nio.channels` you may have encountered an exception called `WritePendingException`. This is a unique exception present in `java.nio.channels` package specifically for AsynchronousFileChannel operations. Today I'll walk you through the details of WritePendingException, its causes, how to handle it and few code samples illustrating the concepts.

## What is WritePendingException in Java?
`WritePendingException` in Java is a type of `IllegalStateException` which is thrown when a write operation is initiated upon a channel, but a previous write has not completed yet. Introduced in JDK 1.7, it's a part of Java's non-blocking I/O (NIO) library which supports asynchronous behavior.

```java
public void writeToChannel(AsynchronousFileChannel afc, ByteBuffer data) throws IOException{
try{
    Future<Integer> result = afc.write(data, 0);
    // Another write operation before the first write completes
    Future<Integer> result2 = afc.write(data, 0);
}catch(WritePendingException e){
    e.printStackTrace();
}}
```
In the code above, we try to initiate a second write operation while the first one is not complete yet, this will result in `WritePendingException`.

## Causes of WritePendingException
The cause of a `WritePendingException` is initiating a write operation on the `AsynchronousFileChannel` while a previous operation is still pending. It's purely a programming error and should be rectified by reordering the operations or by using a callback mechanism to ensure the first operation has finished before starting another one.

## How to Handle WritePendingException Effectively?
In order to handle the `WritePendingException`, we need to ensure not to call another write operation on the `AsynchronousFileChannel` until the previous operation has been completed. This can be achieved in a couple of ways:

### 1. Using Future.get() method 
You can use the `java.util.concurrent.Future.get()` method which waits for the operation to complete before moving towards the next line of execution.
```java
try{
    Future<Integer> result = afc.write(data, 0); 
    result.get();  // waits for the write operation to complete
    Future<Integer> result2 = afc.write(data, 0);  // Now it's OK to initiate second write operation
}catch(Exception e){
    e.printStackTrace();
}
```
### 2. Using CompletionHandler
Another approach can be applying a `CompletionHandler` which is a callback method that gets called once the asynchronous operation is done.
```java
ByteBuffer buffer = ByteBuffer.allocate(1024);
byte[] data = "Hello world!".getBytes();
buffer.put(data);
buffer.flip();
CompletionHandler<Integer, String> handler = new CompletionHandler<>() {
    public void completed(Integer result, String attachment) {
        System.out.println(attachment+ " completed with " + result + " bytes written.");
    }
    public void failed(Throwable e, String attachment) {
        System.out.println(attachment + " failed with: ");
        e.printStackTrace();
    }
};
AsyncFileChannel.write(buffer, 0, "Write_1", handler);
AsyncFileChannel.write(buffer, 0, "Write_2", handler);
```
In this example, two write operations are being executed but they won't overlap due to the `CompletionHandler` in place.  

## Conclusion 
In conclusion, encountering a `WritePendingException` is a reminder to the programmers about an attempt to perform multiple write operations concurrently which is not allowed by the `AsynchronousFileChannel`. It can be prevented by sequentializing the write operations to ensure there's no overlap between them. It's important to note that `WritePendingException` is a runtime exception and cannot be resolved by merely catching the exception at runtime, instead it should be used as a debugging aid during development.

## References
1. [JavaDocs - WritePendingException](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/nio/channels/WritePendingException.html)
2. [Oracle Blog - Asynchronous IO in Java](https://blogs.oracle.com/corejavatechtips/the-constructor-of-filechannel)
3. [Java Docs - AsynchronousFileChannel](https://docs.oracle.com/javase/7/docs/api/java/nio/channels/AsynchronousFileChannel.html) 
4. [Java Docs - CompletionHandler](https://docs.oracle.com/javase/7/docs/api/java/nio/channels/CompletionHandler.html)