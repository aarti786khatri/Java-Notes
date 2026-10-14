# 11. `execute()` vs `submit()`

| `execute()` | `submit()` |
|---|---|
| Accepts a `Runnable` | Accepts `Runnable` or `Callable` |
| Returns `void` | Returns a `Future` |
| No result handle | Can retrieve result or exception through `Future` |
| Uncaught task exceptions are handled by executor/thread uncaught-exception behavior | Task exceptions are captured and reported when `Future.get()` is called |

## Example

```java
ExecutorService executor = Executors.newSingleThreadExecutor();
try {
    executor.execute(() -> System.out.println("No Future returned"));

    Future<Integer> future = executor.submit(() -> 42);
    System.out.println(future.get()); // 42
} catch (InterruptedException e) {
    Thread.currentThread().interrupt();
} catch (ExecutionException e) {
    System.err.println("Task failed: " + e.getCause());
} finally {
    executor.shutdown();
}
```

`Future.get()` blocks until the result is available. Avoid blocking a request thread unnecessarily.
