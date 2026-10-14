# 7. `wait()`, `notify()` and `notifyAll()`

These methods are defined on `Object` and are used for coordination between threads using an object's monitor.

- `wait()`: releases the object's monitor and waits.
- `notify()`: wakes one waiting thread chosen by the JVM.
- `notifyAll()`: wakes all threads waiting on that object's monitor; they must compete to reacquire it.

## Rules

- Call them while owning the same object's monitor, usually inside `synchronized`.
- Always check the condition in a `while` loop, not an `if`, because wake-ups can be spurious and the condition may no longer be true.
- `notify()` does not immediately release the lock. The notifying thread releases it when it exits the synchronized region.

## Producer-consumer style example

```java
class OneSlot {
    private Integer value;

    public synchronized void put(int newValue) throws InterruptedException {
        while (value != null) {
            wait();
        }
        value = newValue;
        notifyAll();
    }

    public synchronized int take() throws InterruptedException {
        while (value == null) {
            wait();
        }
        int result = value;
        value = null;
        notifyAll();
        return result;
    }
}
```

This simple example allows one stored value at a time. Production code often uses `BlockingQueue`, which is usually easier and safer.

## Interview questions

- Why is `wait()` in a `while` loop? To re-check the condition after waking.
- Can `wait()` be called outside synchronized code? It throws `IllegalMonitorStateException`.
- Does `notifyAll()` mean all threads run immediately? No; they must reacquire the monitor first.
