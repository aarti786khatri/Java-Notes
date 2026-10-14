# 17. ExecutorService vs CompletableFuture

They solve related but different problems and can be used together.

| ExecutorService | CompletableFuture |
|---|---|
| Manages execution of submitted tasks and worker threads | Represents a result and lets you compose asynchronous steps |
| `execute()` and `submit()` | `thenApply()`, `thenCompose()`, `thenCombine()`, `exceptionally()` |
| Returns a `Future` from `submit()` | Can build a pipeline of dependent tasks |
| Does not by itself provide fluent result composition | Can use a supplied executor to control where async stages run |

## Example: use a custom executor

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
try {
    CompletableFuture<String> result =
        CompletableFuture.supplyAsync(
            () -> "account data",
            executor
        ).thenApply(String::toUpperCase);

    System.out.println(result.join()); // ACCOUNT DATA
} finally {
    executor.shutdown();
}
```

In real applications, manage executor lifecycle carefully. Avoid creating a new pool for every incoming request.
