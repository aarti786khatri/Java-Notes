# 14. CompletableFuture and Async Methods

## What is CompletableFuture?

`CompletableFuture<T>` represents a result that can be completed later and provides methods to chain, combine and handle asynchronous tasks.
Future has some limitations so we can use this instead of Future# Limitations of Future in Java

In Java, `Future` represents the result of an asynchronous task that may complete in the future.

However, the basic `Future` interface has some limitations. `CompletableFuture` provides additional features to handle these limitations.

## 1. Blocking Retrieval (`get()`)

Calling `future.get()` blocks the calling thread until the result is available.

**Example:**

```java
Future<String> future = executor.submit(() -> "Hello");

String result = future.get(); // Blocks if the result is not ready
System.out.println(result);
```

**Limitation:** The calling thread must wait for the result, which can reduce the benefits of asynchronous programming.

**Better approach:** Use `CompletableFuture` with methods such as `thenApply()` or `thenAccept()` when you want to process a result without explicitly blocking at that point.

## 2. No Built-in Task Chaining

A basic `Future` does not provide methods to automatically execute another task when the first task completes.

**Example scenario:**

1. Fetch customer details.
2. After fetching the details, fetch the customer's orders.

With a basic `Future`, you generally need to manage the steps manually.

**Better approach:** Use `CompletableFuture.thenApply()` or `thenCompose()` to chain tasks.

## 3. No Built-in Completion Callbacks

A basic `Future` does not provide callback methods such as `thenApply()` or `thenAccept()`.

**Limitation:** You cannot directly register a continuation on a basic `Future` to process its result when the task completes.

**Better approach:** Use `CompletableFuture` to register actions that run when a result becomes available.

## 4. Difficult Task Composition

Combining multiple independent or dependent `Future` tasks can be difficult.

**Example scenario:**

An application fetches customer details and account details independently, then combines both results.

With basic `Future`, you may need to call `get()` on each future and combine the results manually.

**Better approach:**

- `CompletableFuture.allOf()` waits for multiple futures to complete.
- `thenCombine()` combines the results of two stages.

## 5. Limited Exception Handling

A basic `Future` does not provide fluent methods for handling errors and recovering from failures.

When calling `get()`, task failures are typically reported through `ExecutionException`. The original cause can be obtained using `getCause()`.

**Better approach:** `CompletableFuture` provides methods such as:

- `exceptionally()` to provide a fallback result.
- `handle()` to process success or failure.
- `whenComplete()` to observe the result or failure.

## 6. No Direct Manual Completion

A basic `Future` returned by `ExecutorService.submit()` does not provide a method to manually supply a result from an external event.

**Example scenario:**

An application needs to complete a result when a message arrives from another system.

**Better approach:** `CompletableFuture` supports manual completion using `complete()` and exceptional completion using `completeExceptionally()`.

## Quick Comparison

| Feature | Future | CompletableFuture |
|---|---|---|
| Retrieve result | `get()` | `get()`, `join()` |
| Task chaining | Not built in | `thenApply()`, `thenCompose()` |
| Completion callbacks | Not built in | `thenAccept()`, `whenComplete()` |
| Combine results | Manual coordination | `thenCombine()`, `allOf()` |
| Exception handling | `ExecutionException` through `get()` | `exceptionally()`, `handle()` |
| Manual completion | Not supported by basic `Future` | `complete()` |

## Interview Answer

**Question: What are the limitations of Future in Java?**

A basic `Future` allows us to retrieve the result of an asynchronous task, but calling `get()` can block the calling thread. It does not provide built-in task chaining, completion callbacks, or convenient methods for combining multiple asynchronous results. Exception handling and manual completion are also less convenient. `CompletableFuture` addresses these limitations by providing chaining, composition, error-handling, and manual-completion methods.

## Important Note

`Future` is still useful when we only need to submit a task and retrieve its result later. `CompletableFuture` is more suitable when we need to build and combine asynchronous workflows.


## Common methods

- `supplyAsync()`: run a task that returns a value.
- `runAsync()`: run a task without a return value.
- `thenApply()`: transform a successful result.
- `thenCompose()`: chain another asynchronous operation that itself returns a stage.
- `thenCombine()`: combine results from two independent stages.
- `thenAccept()`: consume a result without returning another value.
- `exceptionally()`: provide a fallback result after failure.
- `handle()`: process success or failure and return a new result.
- `whenComplete()`: observe success or failure without changing the result in the normal use case.

## Example

```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureExample {
    public static void main(String[] args) {
        CompletableFuture<String> result =
            CompletableFuture.supplyAsync(() -> "java")
                .thenApply(String::toUpperCase);

        System.out.println(result.join()); // JAVA
    }
}
```

## Diagram

```mermaid
flowchart LR
    A[supplyAsync: fetch value] --> B[thenApply: transform]
    B --> C[thenAccept: use result]
```

## Async method names

Methods such as `thenApplyAsync()` usually schedule the action asynchronously, often using the common `ForkJoinPool` unless an executor is supplied. Non-async continuations may run on the thread that completes the previous stage or another thread involved in completion.

For server applications, consider supplying a dedicated executor for blocking I/O and controlling resource usage.

## Interview distinctions

- `thenApply`: transform `T` into `U`.
- `thenCompose`: flatten a nested asynchronous stage.
- `thenCombine`: combine two independent results.
- `join()` throws unchecked `CompletionException` for exceptional completion; `get()` declares checked exceptions.
