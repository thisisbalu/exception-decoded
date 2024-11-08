---
title: "InputMismatchException in Java"
date: 2024-08-09 09:00:00 -0000
categories: [Java, java.base]
tags: [java, java-unchecked, java.util, java-se]
mermaid: true
toc: true
---


Have you ever encountered the InputMismatchException while working with Java? If the answer is yes, then you have come to the right place! In this article, we will discuss the InputMismatchException in Java in detail, understand why it occurs, and learn how to handle it effectively.

## What is InputMismatchException?

InputMismatchException is an exception that is thrown when the next token does not match the expected type or value in the input stream. In simpler terms, this exception is thrown when the input provided by the user does not match the expected input format defined by the program.

Consider the following example:

```java
import java.util.Scanner;

public class InputMismatchExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        try {
            System.out.print("Enter an integer: ");
            int num = scanner.nextInt();
      
            System.out.println("You entered: " + num);
        } catch (InputMismatchException e) {
            System.out.println("Invalid input! Please enter an integer.");
        }
    }
}
```

In the code snippet above, we are using the `nextInt` method of the `Scanner` class to read an integer input from the user. However, if the user enters any non-integer value, such as a string or a floating-point number, the `Scanner` will throw an `InputMismatchException`. To handle this exception, we have enclosed the `nextInt` method within a try-catch block and provided a meaningful error message to the user.

## Common Causes of InputMismatchException

InputMismatchException can occur due to various reasons. Let's take a look at some of the common causes:

1. **Mismatched Data Types:** This exception can be thrown if the data type of the input provided by the user does not match the expected data type defined by the program. For example, if the program expects an integer but the user enters a string, the exception will be thrown.

2. **Incorrect Input Format:** If the user inputs a value that does not match the expected format, such as providing alphabets instead of numeric values or entering a date in an incorrect format, the `Scanner` will throw an InputMismatchException.

3. **Scanner Buffer Mismatch:** Sometimes, the `Scanner` buffer may contain leftover characters or spaces from previous input operations. As a result, when the `Scanner` tries to read the next token, it may encounter an unexpected value, leading to an InputMismatchException.

## Handling InputMismatchException

To handle the InputMismatchException effectively and gracefully, we can make use of try-catch blocks. Let's see how we can handle the exception in different scenarios:

### 1. Handling Mismatched Data Types

```java
try {
    System.out.print("Enter a double value: ");
    double num = scanner.nextDouble();
  
    System.out.println("You entered: " + num);
} catch (InputMismatchException e) {
    System.out.println("Invalid input! Please enter a double value.");
}
```

In the code snippet above, we are using the `nextDouble` method to read a double value from the user. If the user enters an invalid input, such as a string or an integer, an InputMismatchException will be thrown, and the catch block will display an error message.

### 2. Handling Incorrect Input Format

```java
try {
    System.out.print("Enter a date (dd-mm-yyyy): ");
    String date = scanner.nextLine();
  
    // Perform date validation and processing here
  
    System.out.println("Date entered: " + date);
} catch (InputMismatchException e) {
    System.out.println("Invalid input! Please enter the date in the format dd-mm-yyyy.");
}
```

In the code snippet above, we prompt the user to enter a date in the format "dd-mm-yyyy". If the user enters the date in an incorrect format or with invalid characters, an InputMismatchException will be thrown, and the catch block will display an error message.

### 3. Handling Scanner Buffer Mismatch

```java
try {
    System.out.print("Enter an integer: ");
    int num = scanner.nextInt();
  
    System.out.println("You entered: " + num);
  
    // Consume remaining newline character
    scanner.nextLine();
  
    System.out.print("Enter a string: ");
    String str = scanner.nextLine();
  
    System.out.println("You entered: " + str);
} catch (InputMismatchException e) {
    System.out.println("Invalid input! Please enter an integer.");
}
```

In the code snippet above, we first read an integer using the `nextInt` method. After printing the integer, we consume the remaining newline character in the buffer using `scanner.nextLine()`. This ensures that any leftover characters or spaces are cleared from the buffer before reading the next line of input. Then, we prompt the user to enter a string and read it using `scanner.nextLine()`.

## Conclusion

In this article, we explored the InputMismatchException in Java and learned how to handle it effectively. We discussed the common causes of this exception, such as mismatched data types, incorrect input format, and scanner buffer mismatch. By using try-catch blocks, we can gracefully handle the exception and provide meaningful error messages to the users.

Remember, when working with user inputs, it's important to anticipate potential mismatches and handle them gracefully to create robust and user-friendly Java applications.

Keep coding and happy exception handling!

## References

- [Java InputMismatchException Documentation](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/InputMismatchException.html)
- [Java Scanner Documentation](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Scanner.html)
- [Java Exception Handling](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Exception.html)