# 10. ExecutorService

## What is ExecutorService?

`ExecutorService` manages task execution and can reuse a pool of worker threads. Instead of manually creating a new thread for every task, you submit tasks to an executor.

## Example

```java
import java.util.concurrent.*;

public class ExecutorExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);

        try {
            executor.execute(() ->
                System.out.println("Task on " +
                    Thread.currentThread().getName()));
        } finally {
            executor.shutdown();
        }
    }
}
```

## Common methods

- `execute(Runnable)`: submits a task without returning a result handle.
- `submit(...)`: submits a task and returns a `Future`.
- `shutdown()`: stop accepting new tasks; previously submitted tasks can finish.
- `shutdownNow()`: attempts to stop active tasks and returns tasks that never started; it does not guarantee that running tasks stop.
- `awaitTermination(...)`: wait for termination for a limited time.

## Important production note

Choose the pool size and queue capacity based on workload and resource limits. Some `Executors` factory methods create unbounded queues or threads, which may be unsuitable for high-load production systems. Configure `ThreadPoolExecutor` explicitly when you need control over limits and rejection policy.
