---
title: "Dealing with the Ghost: Understanding SkeletonNotFoundException in Java "
date: 2023-11-05 09:00:00 -0000
categories: [Java, java.rmi]
tags: [java, java-unchecked, java.rmi.server, java-se]
mermaid: true
toc: true
---


Java is an incredibly versatile coding language used by millions of developers around the world for everything from creating small apps to building massive enterprise solutions. Among many exceptions that Java developers encounter, *SkeletonNotFoundException* is something that often leaves them scratching their heads. This article aims to clarify what this exception is, when it occurs, and how we can handle it effectively.

## Intro to Remote Method Invocation (RMI)

To comprehend what SkeletonNotFoundException is, we need to dive first into the concept of Java's Remote Method Invocation (RMI). RMI is a Java-only concept that allows an object in one Java Virtual Machine (JVM) to invoke a method of an object existing in another JVM. 

Below is a simplistic example of a RMI server setup:
```java
import java.rmi.*;
import java.rmi.server.*;
public class MyServerImpl extends UnicastRemoteObject implements MyServerInt{
  public MyServerImpl() throws RemoteException {
      super();
   }
  /* method implementation goes here */
}

```
## The SkeletonNotFoundException

Within the realm of RMI, the SkeletonNotFoundException is linked with the concept of *skeletons* or *stubs* that facilitate remote objects communication. 

Before Java 1.2, each remote object to be used with RMI needed two types of special objects - *stubs* and *skeletons*. The *stub* for a remote object is a client-side proxy that forwards the method calls made by the client over the network to the skeleton present at the server. The *skeleton* was responsible for deserializing the incoming request, invoking the method on the actual remote object, and then finally serializering the result before sending it back.

The *SkeletonNotFoundException* is thrown when the RMI runtime is unable to locate a skeleton class for a remote object while marshalling a call.

Here's an example:

```java
try {
    Naming.rebind("rmi://localhost:5000/sonoo",new MyServerImpl());
} catch (Exception e) {
    System.out.println(e);
}
```
If `SkeletonNotFoundException` is encountered here, it means the RMI runtime couldn't find the skeleton class called `MyServerImpl_Skel` for `MyServerImpl` while marshalling a call.

## Leveraging Java 1.2 and Beyond

Good news for the Java developers operating on Java 1.2 and onwards, the explicit use of *skeletons* has been deprecated. RMI now uses a universal, dynamic skeleton to handle calls on remote objects. The stub still exists, but it is automatically generated at runtime, without requiring a separate interface definition.

Thanks to this simplification, the dreaded SkeletonNotFoundException typically doesn't show up. However, you might still encounter it while working with older libraries or upgrading the legacy systems.

## Debugging SkeletonNotFoundException

If you come across the `SkeletonNotFoundException`, here are some things you might explore:

1. **Check Your Classpath:** Ensure the skeletons are correctly located on your classpath. 

2. **Guard Against VersionMismatch:** You may hit a `SkeletonNotFoundException` if you're running your client and server on different versions of Java RMI. Make sure both of them are using the same version.

3. **Compatibility Issues:** If you are using Java 1.2 or later versions, you need to ensure your remote object doesn't implement an interface that extends the now deprecated `java.rmi.server.RemoteStub`. If it does, the RMI runtime will look for the associated skeletons, hence resulting in `SkeletonNotFoundException`.

For better illustration, the following code will cause the `SkeletonNotFoundException` in Java 1.2 or later:

```java
public interface MyRemoteInterface extends java.rmi.server.RemoteStub{
    // method declarations
}
public class MyRemoteObject extends java.rmi.server.UnicastRemoteObject implements MyRemoteInterface{
    // method implementations
}
```

To circumvent this, simply refactor your interface not to extend `RemoteStub`:

```java
public interface MyRemoteInterface extends java.rmi.Remote{
    // method declarations
}
public class MyRemoteObject extends java.rmi.server.UnicastRemoteObject implements MyRemoteInterface{
    // method implementations
}
```

## Conclusion

While SkeletonNotFoundException is not a commonly encountered exception, it can certainly pop up while dealing with some legacy code or maintaining older systems. Recognizing its origin and understanding effective problem-solving strategies can ease your Java journey and help you become a better programmer.

Hopefully, this discussion will clear out the haze around SkeletonNotFoundException and will aid you in your Java exploits.

**References**
1. [Java RMI Documentation](https://docs.oracle.com/javase/tutorial/rmi/)
2. [Java Exception Handling - SkeletonNotFoundException | Baeldung](http://www.baeldung.com/java/exceptions/skeletonnotfoundexception)
3. [Java 1.2 RMI Changes](https://docs.oracle.com/javase/1.5.0/docs/guide/rmi/relnotes.html)
   
## Happy Coding!