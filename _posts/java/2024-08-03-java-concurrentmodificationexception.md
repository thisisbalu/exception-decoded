---
title: "**The Concurrency Conundrum: Understanding and Handling ConcurrentModificationException in Java**"
date: 2024-08-03 09:00:00 -0000
categories: [Java, java.base]
tags: [java, java-unchecked, java.util, java-se]
mermaid: true
toc: true
---


Java, being a highly popular programming language for developing robust and scalable applications, offers great support for creating concurrent programs. However, along with this power, comes the responsibility of managing potential issues that can arise due to concurrent modifications. One such issue is the dreaded `ConcurrentModificationException`, which can wreak havoc in your code if not handled properly.

In this article, we will take an in-depth look at what `ConcurrentModificationException` is, why it occurs, and how to handle it effectively. So, grab a cup of coffee, and let's dive deep into the world of concurrency!

## **Table of Contents**
1. What is `ConcurrentModificationException`?
2. Why does `ConcurrentModificationException` occur?
3. Common scenarios leading to `ConcurrentModificationException`
4. Handling `ConcurrentModificationException`
    - Synchronize access to the collection
    - Using `Iterator` instead of `foreach` loop
    - Using `ConcurrentHashMap` instead of `HashMap`
    - Using `CopyOnWriteArrayList` instead of `ArrayList`
5. Best practices to prevent `ConcurrentModificationException`
6. Conclusion
7. References

## **1. What is `ConcurrentModificationException`?**

`ConcurrentModificationException` is a runtime exception that occurs when a thread attempts to modify a collection while another thread is iterating over it. It signals that the collection has been modified unexpectedly, potentially leading to inconsistent or incorrect behavior of the program.

```java
import java.util.*;

public class ConcurrentModificationExample {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie"));
        
        for (String name : names) {
            if (name.equals("Bob")) {
                names.remove(name); // ConcurrentModificationException occurs here
            }
        }
    }
}
```

In the above code snippet, we have a simple `ArrayList` named `names` containing three names. The loop attempts to remove the name "Bob" from the list while iterating over it. However, this action violates the inherent fail-fast behavior of the `ArrayList`, leading to a `ConcurrentModificationException`.

## **2. Why does `ConcurrentModificationException` occur?**

To understand why `ConcurrentModificationException` occurs, we need to delve into how Java collections handle concurrent modifications.

When an `Iterator` is created for a collection, it keeps track of a modification count that represents the number of structural modifications performed on the collection (e.g., adding or removing elements). This modification count is compared against an expected modification count during each `next()` call on the `Iterator` to ensure that the collection has not been modified since the iterator was created.

If a modification is detected while iterating, the `Iterator` throws a `ConcurrentModificationException` to prevent further iteration and potential data corruption.

## **3. Common scenarios leading to `ConcurrentModificationException`**

Understanding the scenarios that can lead to a `ConcurrentModificationException` helps in identifying potential pitfalls in your code. Here are some common scenarios when this exception can occur:

### 3.1. Modifying a collection during iteration

```java
List<String> fruits = new ArrayList<>(Arrays.asList("Apple", "Banana", "Cherry"));

for (String fruit : fruits) {
    if (fruit.equals("Banana")) {
        fruits.remove(fruit); // ConcurrentModificationException occurs here
    }
}
```

In the above example, we try to remove the element "Banana" from the `ArrayList` while iterating over it. This concurrent modification causes a `ConcurrentModificationException` since the structure of the collection changes during iteration.

### 3.2. Adding elements to a collection during iteration

```java
Set<Integer> numbers = new HashSet<>(Arrays.asList(1, 2, 3, 4));

for (int number : numbers) {
    if (number % 2 == 0) {
        numbers.add(number * 2); // ConcurrentModificationException occurs here
    }
}
```

In this case, we attempt to add elements to a `HashSet` while iterating over it. This concurrent modification leads to a `ConcurrentModificationException` since the structure of the collection changes during iteration. Notably, removing elements from a collection during iteration can also trigger this exception.

## **4. Handling `ConcurrentModificationException`**

Now that we understand the causes of the `ConcurrentModificationException`, it's time to explore some ways to handle it effectively.

### 4.1. Synchronize access to the collection

One straightforward approach is to synchronize access to the collection and explicitly lock it during iteration and modification. This ensures that only one thread can modify the collection at a time, avoiding conflicts and `ConcurrentModificationException`.

```java
List<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie"));

synchronized (names) {
    Iterator<String> iterator = names.iterator();
    
    while (iterator.hasNext()) {
        String name = iterator.next();
        
        if (name.equals("Bob")) {
            iterator.remove(); // Safely remove element from the collection
        }
    }
}
```

In the above example, we wrap the iteration code within a synchronized block, locking the `names` list while iterating over it. This ensures that no other thread can modify the list concurrently, thus preventing `ConcurrentModificationException`.

### 4.2. Using `Iterator` instead of `foreach` loop

Another way to handle `ConcurrentModificationException` is by using the `Iterator` directly instead of the `foreach` loop. The `Iterator` provides a safe mechanism to iterate and modify collections concurrently without throwing the `ConcurrentModificationException`.

```java
List<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie"));

Iterator<String> iterator = names.iterator();

while (iterator.hasNext()) {
    String name = iterator.next();
    
    if (name.equals("Bob")) {
        iterator.remove(); // Safely remove element from the collection
    }
}
```

By using the `Iterator` explicitly, we can safely remove elements from the collection without encountering the `ConcurrentModificationException`. The `Iterator` adapts to any modification made during iteration, guaranteeing a consistent view of the collection.

### 4.3. Using `ConcurrentHashMap` instead of `HashMap`

In scenarios where the underlying collection needs to support concurrent modifications efficiently, using `ConcurrentHashMap` instead of `HashMap` can be a viable solution. Unlike `HashMap`, `ConcurrentHashMap` provides a built-in concurrency support mechanism, allowing multiple threads to modify the collection without throwing `ConcurrentModificationException`.

```java
Map<String, Integer> scores = new ConcurrentHashMap<>();
scores.put("Alice", 95);
scores.put("Bob", 80);
scores.put("Charlie", 90);

for (String name : scores.keySet()) {
    if (scores.get(name) < 90) {
        scores.remove(name); // Safe concurrent modification
    }
}
```

In the above code snippet, we iterate over the keys of a `ConcurrentHashMap` and remove keys with values less than 90. This modification is performed concurrently, thanks to the built-in concurrency support of `ConcurrentHashMap`, eliminating any possibilities of `ConcurrentModificationException`.

### 4.4. Using `CopyOnWriteArrayList` instead of `ArrayList`

Similarly, if you're dealing with a scenario where the collection needs to support concurrent modifications efficiently, consider using the `CopyOnWriteArrayList` class. It provides thread-safe iteration and modification by creating an entirely new copy of the underlying array whenever a modification is performed, ensuring a consistent and fail-safe view of the collection.

```java
List<String> cities = new CopyOnWriteArrayList<>(Arrays.asList("New York", "London", "Paris"));

for (String city : cities) {
    if (city.equals("London")) {
        cities.remove("London"); // Safe concurrent modification
    }
}
```

In this example, we remove the city "London" from a `CopyOnWriteArrayList` while iterating over it. The `CopyOnWriteArrayList` handles the modification by creating a fresh copy of the underlying array, allowing safe and concurrent modifications without throwing `ConcurrentModificationException`.

## **5. Best practices to prevent `ConcurrentModificationException`**

Here are some best practices to keep in mind when working with concurrent modifications to prevent `ConcurrentModificationException`:

- Use appropriate synchronization mechanisms, such as `synchronized` blocks or concurrent data structures, to serialize the modifications and iterations on the shared collections.
- Prefer `Iterator` over `foreach` loop when modifying collections while iterating, as `Iterator` provides a safe mechanism to handle concurrent modifications without throwing `ConcurrentModificationException`.
- Avoid modifying the collection during iteration as much as possible. Instead, collect the modifications in a separate data structure and perform them outside the iteration loop.
- Design your code with proper synchronization policies and thread-safe data structures from the beginning to minimize the chances of encountering `ConcurrentModificationException`.
- Thoroughly test your code for concurrent modifications, either manually or by employing concurrency testing frameworks like JUnit's `@Test(expected = ConcurrentModificationException.class)`.

These best practices will help you write robust and concurrent code, eliminating unexpected `ConcurrentModificationException` from your programs.

## **6. Conclusion**

Managing concurrent modifications is critical when dealing with multi-threaded Java applications. The `ConcurrentModificationException` serves as a reminder to Java developers to handle concurrent modifications effectively and prevent data corruption or inconsistencies.

In this article, we explored `ConcurrentModificationException` in depth, understanding its causes, and various techniques to handle it. By using synchronization, employing `Iterator`, and leveraging concurrent data structures provided by Java, you can effectively tackle concurrent modifications and ensure the correctness of your concurrent programs.

So next time you encounter a `ConcurrentModificationException`, embrace it as an opportunity to strengthen your understanding of concurrency and apply the suitable techniques discussed here to combat it!

## **7. References**

1. [Java API Documentation - ConcurrentModificationException](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/ConcurrentModificationException.html)
2. [Java Tutorials - "The Collection Interface"](https://docs.oracle.com/javase/tutorial/collections/interfaces/collection.html)
3. [Java Tutorials - "The Map Interface"](https://docs.oracle.com/javase/tutorial/collections/interfaces/map.html)
4. [Java Tutorials - "The CopyOnWriteArrayList Class"](https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/CopyOnWriteArrayList.html)

---
*Estimated reading time: 15 minutes*