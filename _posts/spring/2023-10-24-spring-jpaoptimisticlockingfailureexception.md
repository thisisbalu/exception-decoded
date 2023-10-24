---
title: "Solving Mysteries of JpaOptimisticLockingFailureException in Spring Framework"
date: 2023-10-31 09:00:00 -0000
categories: [Spring, spring-framework]
tags: [spring, spring-unchecked, org.springframework.orm.jpa]
mermaid: true
toc: true
---


Do you keep getting a `JpaOptimisticLockingFailureException` while working with the Spring Framework? Don't you worry! This exception, although initially intimidating, can be resolved using smart, effective coding strategies. In this comprehensive guide, we will dig into the nitty-gritty of `JpaOptimisticLockingFailureException`, why it occurs, and how to handle it effectively. 

## Table of contents

1. Understanding JpaOptimisticLockingFailureException
2. Why it happens
    1. Unmanaged concurrent access
    2. Stale data in session state
3. Handling JpaOptimisticLockingFailureException
    1. Using Optimistic locking
    2. Using Pessimistic locking
4. Examples
5. Conclusion

## 1. Understanding JpaOptimisticLockingFailureException

`JpaOptimisticLockingFailureException` is a `javax.persistence.OptimisticLockException` sub-type, specific to JPA-based data access. Spring's DataAccessException hierarchy bridges the gap between various persistence technologies like JDBC, Hibernate, JPA or JDO and Spring's exception handling strategy, allowing the Spring data access framework exceptions to get translated seamlessly to the technology specific exceptions like JpaOptimisticLockingFailureException.

```Java
public class JpaOptimisticLockingFailureException 
    extends OptimisticLockingFailureException
...
```

## 2. Why It Happens

Before we dive into solutions, let's clarify why you'd encounter this exception in the first place. The following are common causes:

### A. Unmanaged concurrent access

Multiple threads are concurrently accessing and changing the same data, leading to conflicts that throw JpaOptimisticLockingFailureException.

```Java
// Multiple threads trying to access same entity
Thread thread1 = new Thread(() -> {
    repository.findById(id).ifPresent(entity -> {
        entity.changeData();
        repository.save(entity);
    });
});

Thread thread2 = new Thread(() -> {
    repository.findById(id).ifPresent(entity -> {
        entity.changeData();
        repository.save(entity);
    });
});

thread1.start();
thread2.start();
```

### B. Stale data in session state:

An entity fetched from the database at a certain point in time reflects the state of the database rows at that moment. Over time, other transactions could modify those rows, but your entity wouldn't know about those changes. This stale state could cause `JpaOptimisticLockingFailureException`. 

```Java
Entity originalEntity = repository.findById(id).get();
...
// Over time, state of originalEntity becomes stale as database rows change
Entity staleEntity = originalEntity;
...
repository.save(staleEntity); // Throws JpaOptimisticLockingFailureException
```

## 3. Handling JpaOptimisticLockingFailureException

Spring provides two different ways to solve the `JpaOptimisticLockingFailureException`, one by being optimistic (Optimistic Locking) and the other by being pessimistic (Pessimistic Locking).

### A. Using Optimistic Locking 

Optimistic Locking relies on checks made at transaction commit time to verify that the data involved in the transaction hasn't changed since it was read.

```java
@Version
private Integer version;
```

### B. Using Pessimistic Locking 

Pessimistic Locking involves using database-specific locking mechanisms to lock data from modifications as soon as it is read. 

```java
@Lock(LockModeType.PESSIMISTIC_WRITE)
Entity getEntityForUpdate(Long id);
```

## 4. Examples

Let's look at how to use these strategies with code examples:

```java
@Entity
public class MyEntity {
    ...
    @Version
    private Long version;
    ...
}

@Service
public class MyEntityService {
    
    @Autowired
    private MyEntityRepository repository;
    
    public void updateEntity(MyEntity entity) {
        try {
            repository.save(entity);
        } catch (JpaOptimisticLockingFailureException ex) {
            ...
        }
    }
}

@Repository
public interface MyEntityRepository extends JpaRepository<MyEntity, Long> {
    @Lock(LockModeType.PESSIMISTIC_WRITE)
    MyEntity getEntityForUpdate(Long id);
}
```

## 5. Conclusion

Handling `JpaOptimisticLockingFailureException` requires a good understanding of your application's behavior, especially concerning data access and multi-threading. Both optimistic and pessimistic locking have their pros and cons, but with Spring's powerful support for data access management, the choice ultimately depends on your specific requirements and scenarios.

Reference:
- [Spring Optimistic Locking](https://www.baeldung.com/jpa-optimistic-locking)
- [Spring Pessimistic Locking](https://www.baeldung.com/jpa-pessimistic-locking)
- [JpaOptimisticLockingFailureException documentation](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/dao/JpaOptimisticLockingFailureException.html) 

Happy coding!