---
title: "When Java refuses to move: Unpacking the 'AtomicMoveNotSupportedException'"
date: 2023-09-30 09:00:57 -0000
categories: [Java, java.base]
tags: [java, java-checked, java.nio.file, java-se]
mermaid: true
toc: true
---


Among the assortment of errors and exceptions that a developer may encounter in Java, the `AtomicMoveNotSupportedException` is one that you may bump into when dealing with filesystem operations, particularly during file transfer operations. This article aims to deep-dive into this exception, its causes and, signalling our important catch here, suggesting practical ways to navigate around it. Welcome to another journey into the heart of Java. Let's decode the `AtomicMoveNotSupportedException`.

## Unmasking the `AtomicMoveNotSupportedException`

So, what is the `AtomicMoveNotSupportedException`? Simply put, this exception occurs when the Java Virtual Machine (JVM) or the underlying OS file system does not support atomic file move operations or renames.

Atomic, what? We're not talking about atoms or physics here, but atomicity in the world of computing which refers to a series of operations that seemingly occur as one indivisible, uninterrupted operation. It either entirely completes or fails, without leaving any intermediate state. 

When we're undertaking a file move operation in Java, `AtomicMoveNotSupportedException` can become a speed-breaker if the JVM or the underlying file system doesn't facilitate such uninterrupted file move operations.

Consider this simple code example:

```java
Path sourcePath = Paths.get("/Users/path/source.txt");
Path targetPath = Paths.get("/Users/path/destination.txt");

Files.move(sourcePath, targetPath, StandardCopyOption.ATOMIC_MOVE);
```

If you run this snippet in an environment that does not support atomic move operations, Java will raise an `AtomicMoveNotSupportedException`.

## Pinning the Culprit 

The underlying problem here is not with Java, but the different file systems that Java operates in tandem with. Atomic move feature is not universal, and some file systems simply don't support it. For instance, older versions of Windows and certain Unix-based file systems might trigger this exception.

There are two key actors in this drama:

1. **Your Java Virtual Machine (JVM):** Different versions and implementations of JVMs may have different levels of support for file system operations. 

2. **The underlying Operating System (OS):** The file system used by the OS can be the deciding factor for support of atomic move operations.

Ergo, if your JVM or OS don’t support atomic moves or if you're trying to move a file between file stores that don't have cross-file store atomic moves support, you'll get an `AtomicMoveNotSupportedException`.

## Working Around the Exception

While we can't alter an OS-level feature, we can write code that considers it. Here's how we can navigate around this:

### A. Check for the atomic move support:

Before running the move operation, you should check if your file store supports it. Here's how:

```java
FileSystem fs = FileSystems.getDefault();
for(FileStore store: fs.getFileStores()){
    boolean supportsAtomicMove = Files.getFileStore(Paths.get(".")).supportsFileAttributeView("basic");
    System.out.println(store + ": " + supportsAtomicMove);
}
```

This will print true if the file store supports atomic moves and false if it doesn't.

### B. Fallback to non-atomic move:

As a best practice, you should always catch `AtomicMoveNotSupportedException` when performing atomic move operations. In the catch block, you can fallback to a standard, non-atomic move operation. Refer to the code below:

```java
Path sourcePath = Paths.get("source_path");
Path targetPath = Paths.get("target_path");

try {
     Files.move(sourcePath, targetPath, StandardCopyOption.ATOMIC_MOVE);
} catch(AtomicMoveNotSupportedException e) {
     Files.move(sourcePath, targetPath);
}
```

## Key Takeaways

This article offers a quick walkthrough of `AtomicMoveNotSupportedException` in Java. Programming is about knowing the quirks of your terrain and navigating around them. While atomic moves might seem like a boon, but their limited support can bring unexpected errors. The best approach is to always check for support of atomic operations in your file store, and include a fallback option ready in your arsenal.

With this, you’re better equipped to deal with the `AtomicMoveNotSupportedException` in Java. Happy coding!

### References:

1. [Java 7 Files Class Documentation](https://docs.oracle.com/javase/7/docs/api/java/nio/file/Files.html)
2. [Understanding AtomicMoveNotSupportedException](https://stackoverflow.com/questions/18338770/java-nio-file-how-to-correctly-handle-atomicmovenotsupportedexception)
3. [Java class AtomicMoveNotSupportedException](https://www.programcreek.com/java-api-examples/?api=java.nio.file.AtomicMoveNotSupportedException)