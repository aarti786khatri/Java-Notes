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

1. volatile keyword
When to use: When one thread updates a flag and another thread needs to see the latest value.
Real-time example: Stopping a background worker.
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


Why volatile? It ensures that changes to running are visible to other threads. Without it, visibility is not guaranteed.
Remember: volatile does not make operations like count++ atomic.
2. AtomicInteger
When to use: When multiple threads update a counter and each update must be atomic.
Real-time example: Counting how many requests an API receives.
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


Why AtomicInteger? Its incrementAndGet() method performs the increment atomically, so concurrent updates are not lost.
Remember: Use atomic classes for individual atomic operations. For multiple related operations that must happen together, you may need synchronization or another coordination mechanism.
3. synchronized keyword
When to use: When multiple threads access shared data and a group of operations must be protected by the same lock.
Real-time example: Two threads withdrawing money from the same bank account.
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


Why synchronized? Only one thread at a time can execute this method on the same account object. This protects the balance check and deduction as one critical section.
For example, if two threads try to withdraw ₹700, only one should succeed when the starting balance is ₹1,000.
Quick interview comparison
Feature	Best use	Example
volatile	Visibility of a shared variable	Stop-worker flag
AtomicInteger	Thread-safe counter updates	API request count
synchronized	Protecting a group of operations	Bank withdrawal
Interview tip: If the interviewer asks whether volatile int count makes count++ thread-safe, answer no. The increment involves reading, adding, and writing. Use AtomicInteger for an atomic increment or synchronized when multiple operations must be protected together.
