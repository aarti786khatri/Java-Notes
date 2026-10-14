# 13. Future

## What is Future?

`Future<T>` represents the result of an asynchronous computation that may finish later.

It lets you:
- Check whether the task is done using `isDone()`.
- Wait for a result using `get()`.
- Attempt cancellation using `cancel(...)`.

## Example

```java
ExecutorService executor = Executors.newSingleThreadExecutor();
try {
    Future<String> future = executor.submit(() -> {
        Thread.sleep(500);
        return "Report ready";
    });

    System.out.println(future.get());
} catch (InterruptedException e) {
    Thread.currentThread().interrupt();
} catch (ExecutionException e) {
    throw new RuntimeException(e.getCause());
} finally {
    executor.shutdown();
}
```

Output:
```text
Report ready
```

## Limitations

- `get()` blocks the calling thread until the result is ready.
- Combining multiple futures can be awkward.
- It does not provide the rich composition methods available in `CompletableFuture`.

Cancellation is an attempt, not a guarantee. A task must cooperate with interruption or cancellation for it to stop promptly.
