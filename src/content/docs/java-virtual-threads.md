---
title: "Understanding Java Virtual Threads"
description: "Java 21 virtual threads fundamentally change how we write concurrent code. Here's what you need to know."
pubDate: 2026-01-10
tags: ["java", "concurrency"]
---

## Platform Threads vs Virtual Threads

Traditional Java threads (platform threads) are 1:1 mapped to OS threads. Each one costs ~1MB of stack memory. Virtual threads are lightweight — you can create **millions** of them.

```java
// Old way: limited by OS thread count
ExecutorService executor = Executors.newFixedThreadPool(200);

// New way: virtually unlimited
ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor();
```

## When to Use Virtual Threads

Virtual threads shine for **I/O-bound** workloads:

- HTTP request handling
- Database queries
- File I/O operations
- External API calls

They are **not** beneficial for CPU-bound tasks like image processing or complex calculations.

## Spring Boot Integration

Spring Boot 3.2+ supports virtual threads with a single property:

```properties
spring.threads.virtual.enabled=true
```

This switches Tomcat to use virtual threads for request handling. Each incoming request gets its own virtual thread.

## Structured Concurrency (Preview)

Java 21 also introduces structured concurrency for managing related tasks:

```java
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    Subtask<User> user = scope.fork(() -> fetchUser(id));
    Subtask<Order> order = scope.fork(() -> fetchOrder(id));

    scope.join().throwIfFailed();

    return new UserProfile(user.get(), order.get());
}
```

## Key Differences

| Feature | Platform Threads | Virtual Threads |
|---------|-----------------|-----------------|
| Memory | ~1MB each | ~few KB each |
| Count | Thousands | Millions |
| Scheduling | OS scheduler | JVM scheduler |
| Best for | CPU-bound | I/O-bound |
| Pooling needed | Yes | No |

## Gotchas

- **Don't pool virtual threads** — create new ones per task
- **Avoid `synchronized`** — use `ReentrantLock` instead (synchronized pins the carrier thread)
- **Thread locals work** but consume memory per virtual thread
