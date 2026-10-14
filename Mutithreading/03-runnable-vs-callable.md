# 3. Runnable vs Callable

Both represent work that can be executed by another thread or an executor.

| Runnable | Callable<V> |
|---|---|
| `run()` method | `call()` method |
| Does not return a result | Can return a result |
| Cannot declare checked exceptions from `run()` | `call()` can throw checked exceptions |
| Commonly used for tasks without a result | Useful when a task returns a value |

## Runnable example

```java
Runnable task = () -> System.out.println("Sending email");
new Thread(task).start();
```

## Callable with ExecutorService

```java
import java.util.concurrent.*;

public class CallableExample {
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newSingleThreadExecutor();
        try {
            Callable<Integer> task = () -> 10 + 20;
            Future<Integer> result = executor.submit(task);
            System.out.println(result.get()); // 30
        } finally {
            executor.shutdown();
        }
    }
}
```

`submit()` returns a `Future`. Calling `get()` waits if the result is not ready yet.

## Interview example

Use `Runnable` for a task such as writing a log message. Use `Callable<Integer>` when a background task must calculate and return a value.
