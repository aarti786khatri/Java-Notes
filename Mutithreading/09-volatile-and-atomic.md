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

# Volatile vs AtomicInteger vs Synchronized

## 1. Volatile Keyword

### When to use?
Use `volatile` when one thread updates a variable and other threads need to see the latest value.

**Real-time example:** Stopping a background worker.

```java
class Worker {
    private volatile boolean running = true;

    public void work() {
        while (running) {
            // Do background work
        }
        System.out.println("Worker stopped");
    }

    public void stop() {
        running = false;
    }
}
```

### Why use volatile?
- It makes changes visible to other threads.
- It is useful for status flags.
- It does not make operations like `count++` atomic.

---

## 2. AtomicInteger

### When to use?
Use `AtomicInteger` when multiple threads need to update a counter safely.

**Real-time example:** Counting API requests.

```java
import java.util.concurrent.atomic.AtomicInteger;

class ApiCounter {
    private final AtomicInteger count = new AtomicInteger(0);

    public void requestReceived() {
        count.incrementAndGet();
    }

    public int getCount() {
        return count.get();
    }
}
```

### Why use AtomicInteger?
- `incrementAndGet()` performs an atomic increment.
- Concurrent updates are not lost.
- It is useful for counters and simple atomic updates.

---

## 3. Synchronized Keyword

### When to use?
Use `synchronized` when multiple threads access shared data and multiple operations must be protected together.

**Real-time example:** Two threads withdrawing money from the same bank account.

```java
class BankAccount {
    private int balance = 1000;

    public synchronized void withdraw(int amount) {
        if (balance >= amount) {
            balance -= amount;
            System.out.println("Remaining balance: " + balance);
        } else {
            System.out.println("Insufficient balance");
        }
    }
}
```

### Why use synchronized?
- Only one thread at a time can execute this method on the same account object.
- It protects the balance check and deduction together.
- It helps prevent race conditions.

---

## Quick Comparison

| Feature | Purpose | Example |
|---|---|---|
| `volatile` | Visibility | Worker stop flag |
| `AtomicInteger` | Atomic counter update | API request count |
| `synchronized` | Protect multiple operations | Bank withdrawal |

## Interview Questions

1. Does `volatile` make `count++` thread-safe? No.
2. When should we use `AtomicInteger`? For atomic counter updates.
3. When should we use `synchronized`? When a critical section needs protection.
4. Can `volatile` replace `synchronized`? Not when mutual exclusion or atomic multi-step operations are required.

## Easy Way to Remember

- `volatile` = Visibility
- `AtomicInteger` = Atomic update
- `synchronized` = One thread at a time for the same lock




