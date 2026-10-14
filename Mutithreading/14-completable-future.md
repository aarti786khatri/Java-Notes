# 14. CompletableFuture and Async Methods

## What is CompletableFuture?

`CompletableFuture<T>` represents a result that can be completed later and provides methods to chain, combine and handle asynchronous tasks.

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
