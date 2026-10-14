# 8. `sleep()` vs `wait()`

| `Thread.sleep()` | `Object.wait()` |
|---|---|
| Static method of `Thread` | Instance method of `Object` |
| Pauses current thread for a duration | Waits for notification or timeout |
| Does not release monitors held by the thread | Releases the monitor of the object it waits on |
| Does not require owning a monitor | Must own that object's monitor |
| Commonly used for timed pause | Used for thread coordination |

## Example

```java
Thread.sleep(1000); // pause current thread for about 1 second
```

For `wait()`, use a synchronized monitor and a condition loop:

```java
synchronized (lock) {
    while (!condition) {
        lock.wait();
    }
}
```

`InterruptedException` is checked, so handle it or declare it. If you cannot propagate it, commonly restore the interrupt status with `Thread.currentThread().interrupt()` before returning or stopping the operation.
