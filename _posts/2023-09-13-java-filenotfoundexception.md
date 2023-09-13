---
title: "Tackling FileNotFoundException in Java: A Comprehensive Guide"
description: "Uncover the excruciating mystery of FileNotFoundException in Java with the elaborated explanation & real-time code examples."
date: 2023-09-13 13:10:00 +0800
categories: ['Java', 'java.io']
tags: [java, checked-exception]
mermaid: true
toc: true
---

Java is an object-oriented programming language that has a strong exception handling mechanism to resolve any issues during the execution of code. Java provides several predefined exceptions to handle special events that occur during the execution of the code. Today, we will delve into one such exception - The FileNotFoundException in Java.

## Understanding FileNotFoundException

**FileNotFoundException** is a [checked exception](https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html) in Java - which is thrown by the Java Virtual Machine (JVM) during the runtime when a file that is supposed to be read or written to doesn't exist in the specified location.

### When does FileNotFoundException occur?

1. The file doesn’t exist in the mentioned directory.
2. The file exists, but the user has insufficient permissions to access the file.
3. The file is a directory rather than a regular file.

### How FileNotFoundException Works?

Below is a simple instance to understand FileNotFoundException:

```java
import java.io.File;
import java.io.FileReader;

public class Main {
    public static void main(String[] args) {
        fileOperation();
    }

    static void fileOperation() {
        File file = new File("NoFile.txt");
        FileReader fr = new FileReader(file);
    }
}
```

This code attempts to create a FileReader object with a file named "NoFile.txt". Since the file does not exist, Java throws a FileNotFoundException.

## Handling FileNotFoundException in Java

Java endorses a robust approach to deal with exceptions via try-catch mechanism.

Below is a simple code example :

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;

public class Main {
    public static void main(String[] args) {
        try {
            fileOperation();
        }
        catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }

    static void fileOperation() throws FileNotFoundException {
        File file = new File("NoFile.txt");
        FileReader fr = new FileReader(file);
    }
}
```

In this code, the `fileOperation()` method is wrapped inside a try-catch block. The catch block handles the FileNotFoundException.

### "throws" Keyword

Java also allows the use of the `throws` keyword to delegate exception handling to the calling method. This keyword can be used to throw a single exception or multiple exceptions.

```java
import java.io.*;

public class Main {
    public static void main(String[] args) {
        try {
            fileOperation();
        }
        catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        catch(IOException e){
            e.printStackTrace();
        }
    }

    static void fileOperation() throws FileNotFoundException, IOException {
        File file = new File("NoFile.txt");
        FileReader fr = new FileReader(file);
        fr.close();
    }
}
```
In this code, the `fileOperation()` method is capable of throwing both FileNotFoundException and IOException. IOException handles any issue that happens while opening or closing the file.


## Best Practices to handle FileNotFoundException

### 1. Check if a file Exists

It is recommended to check the existence of a file before trying to read or write.

```java
File file = new File("NoFile.txt");
if(file.exists()){
   // Perform operations on file
}
```

### 2. Give Proper File Locations

Ensure that you are providing the full and correct path of the file.

```java
File file = new File("/home/user/documents/file.txt");
```

### 3. Check Access Permissions

Ensure that the appropriate permissions are granted to the file.

```java
File file = new File("file.txt");
if(file.canRead()){
   // Perform operations on file
}
```

## Precautionary Steps

✔️ Before trying to read from or write to a file, use the `exists()` method from the `java.io.File` class to check file existence.

✔️ Handle file access in a robust manner, accounting for all possible edge cases.

✔️ If required, use `java.io.File` class’s `canRead()`, `canWrite()`, `canExecute()` to verify file permissions.

✔️ Check if the file in question is, in actuality, a directory with the `isDirectory()` method.


In conclusion, FileNotFoundException in Java is an exception that a developer cannot afford to overlook while dealing with file operations. For further understanding, you should refer to the [official Java documentation](https://docs.oracle.com/en/java/javase/14/docs/api/index.html?java/io/FileNotFoundException.html).

So next time a file is playing hide and seek in your Java code, you know how to catch it!

## References
1. [Oracle Java Documentations](https://docs.oracle.com/en/java/javase/14/docs/api/index.html?java/io/FileNotFoundException.html)
2. [Java Exception Handling](https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html)

Happy Coding!
