# 15. Interrupt and Daemon Threads

## `interrupt()`

Interrupting a thread is a cooperative signal asking it to stop or change what it is doing. It does not forcibly kill the thread.

```java
class Worker implements Runnable {
    public void run() {
        while (!Thread.currentThread().isInterrupted()) {
            // Do a small unit of work
        }
        System.out.println("Worker stopping");
    }
}
```

If a blocking method such as `sleep()` throws `InterruptedException`, the interrupt status is cleared as part of throwing the exception. If you cannot propagate the exception, restore the status:

```java
catch (InterruptedException e) {
    Thread.currentThread().interrupt();
    return;
}
```

## Daemon thread

A daemon thread is a background service thread. The JVM can exit when only daemon threads remain.

```java
Thread t = new Thread(() -> {
    while (true) {
        // background work
    }
});
t.setDaemon(true); // must be called before start()
t.start();
```

Do not use daemon threads for work that must finish reliably, such as saving critical data. The JVM does not wait for daemon threads to complete when non-daemon threads have finished.

## Interview questions

- Does `interrupt()` immediately stop a thread? No.
- Can `setDaemon(true)` be called after `start()`? No.
- Should we swallow `InterruptedException`? Usually no; propagate it or restore the interrupt status and exit/cooperate.
