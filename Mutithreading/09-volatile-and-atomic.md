# 9. `volatile` and Atomic Variables

## `volatile`

`volatile` makes writes to a variable visible to other threads that subsequently read that variable. It also establishes relevant memory-ordering guarantees.
It will read and update value in/from in memory.

```java
class Worker {
    private volatile boolean running = true;

    void stop() {
        running = false;
    }

    void work() {
        while (running) {
            // Do a small unit of work
        }
    }
}
```

Without appropriate visibility guarantees, a thread might not observe an update promptly.

**Important:** `volatile` does not make compound operations such as `count++` atomic.

## AtomicInteger

```java
AtomicInteger count = new AtomicInteger();
count.incrementAndGet();
```

Atomic classes provide thread-safe atomic operations for supported updates, commonly using compare-and-set techniques.

## Which should I use?

- Use `volatile` for a simple shared state flag when the operation does not depend on an atomic compound update.
- Use `AtomicInteger` for atomic counter updates.
- Use `synchronized` or locks when several operations must be protected together as one critical section.


