
# ReentrantLock in Java

## 1. What is ReentrantLock?

`ReentrantLock` is a class in Java that provides explicit locking to control access to shared resources when multiple threads are running.

It belongs to the `java.util.concurrent.locks` package.

**Simple meaning:** It allows only one thread at a time to hold a particular lock, protecting a critical section from concurrent access.

### What does Reentrant mean?

Reentrant means **the same thread can acquire the same lock multiple times** without getting blocked by its own lock.

For example, if Thread A acquires a lock and calls another method that acquires the same lock, Thread A can acquire it again. It must release the lock the same number of times it acquired it.

## 2. Real-time example

Imagine two threads trying to withdraw money from the same bank account.

We need to protect the balance check and deduction so that both threads cannot perform these operations at the same time.

## 3. Simple Java program

```java
import java.util.concurrent.locks.ReentrantLock;

class BankAccount {

    private int balance = 1000;

    private final ReentrantLock lock = new ReentrantLock();

    public void withdraw(int amount) {
        lock.lock();

        try {
            if (balance >= amount) {
                balance -= amount;
                System.out.println(
                    Thread.currentThread().getName()
                    + " withdrew " + amount
                    + ", remaining balance: " + balance
                );
            } else {
                System.out.println("Insufficient balance");
            }
        } finally {
            lock.unlock();
        }
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();

        Thread t1 = new Thread(
            () -> account.withdraw(700), "Thread-1"
        );

        Thread t2 = new Thread(
            () -> account.withdraw(700), "Thread-2"
        );

        t1.start();
        t2.start();
    }
}
```

### Possible output

```text
Thread-1 withdrew 700, remaining balance: 300
Insufficient balance
```

The thread order can vary. Only one withdrawal can execute the protected balance check and update at a time.

## 4. How does the code work?

1. `lock.lock()` acquires the lock. If another thread owns it, the current thread waits.
2. The `try` block checks the balance and withdraws money.
3. The `finally` block calls `lock.unlock()`, releasing the lock even if an exception occurs inside the `try` block.
4. The other thread can then acquire the lock and check the updated balance.

**Important:** Always release a lock in a `finally` block. Otherwise, an exception could leave the lock held and prevent other threads from progressing.

## 5. Important methods of ReentrantLock

| Method | Purpose |
|---|---|
| `lock()` | Acquires the lock, waiting if necessary |
| `unlock()` | Releases one hold on the lock |
| `tryLock()` | Attempts to acquire the lock without waiting |
| `tryLock(time, unit)` | Waits up to a specified time to acquire the lock |
| `lockInterruptibly()` | Allows waiting for the lock to be interrupted |
| `isLocked()` | Checks whether the lock is currently held by any thread |

## 6. ReentrantLock vs synchronized

| `synchronized` | `ReentrantLock` |
|---|---|
| Built into Java syntax | Class from `java.util.concurrent.locks` |
| Lock is released automatically when the synchronized block exits | You must call `unlock()` explicitly |
| No timed lock-acquisition method | Supports timed `tryLock()` |
| Cannot interrupt a thread specifically while it is waiting to enter the monitor | `lockInterruptibly()` supports interruptible acquisition |
| Simpler for basic locking | Useful when more lock-control features are needed |

Both provide mutual exclusion and memory-visibility guarantees when used correctly. `ReentrantLock` is not automatically faster than `synchronized`.

## 7. Example of reentrancy

```java
import java.util.concurrent.locks.ReentrantLock;

class Demo {
    private final ReentrantLock lock = new ReentrantLock();

    public void method1() {
        lock.lock();
        try {
            System.out.println("Inside method1");
            method2();
        } finally {
            lock.unlock();
        }
    }

    public void method2() {
        lock.lock();
        try {
            System.out.println("Inside method2");
        } finally {
            lock.unlock();
        }
    }
}
```

Here, `method1()` acquires the lock and calls `method2()`. The same thread can acquire the same lock again.

Both acquisitions are released by their respective `unlock()` calls.

## 8. Interview questions

**Q1. Why is it called ReentrantLock?**

Because the same thread can acquire the same lock multiple times.

**Q2. What happens if we forget to call `unlock()`?**

Other threads may wait indefinitely to acquire the lock.

**Q3. What is the benefit of `tryLock()`?**

It lets a thread attempt to acquire the lock without waiting indefinitely. The code must check whether acquisition succeeded before accessing the protected resource.

**Q4. Can two threads acquire the same ReentrantLock simultaneously?**

No. Only one thread can own the lock at a time.

**Q5. When would you use ReentrantLock instead of synchronized?**

When I need features such as timed lock acquisition, interruptible acquisition, or more explicit control over locking.

## Quick revision

- `ReentrantLock` provides explicit locking.
- The same thread can acquire the same lock repeatedly.
- Use `try-finally` to ensure the lock is released.
- `tryLock()` can help avoid waiting indefinitely.
- Prefer `synchronized` for simple cases; use `ReentrantLock` when its extra features are useful.
