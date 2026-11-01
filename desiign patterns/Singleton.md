
# Singleton Design Pattern in Java

## 1. What is the Singleton Design Pattern?

Singleton is a **creational design pattern** in Java.

It ensures that:
- Only one instance of a class is created.
- The same instance is shared wherever it is needed.
- The class provides a single point of access to that instance.

### Real-time example

Think about a logging service in an application.

We generally don't need to create a new logging service object every time we want to write a log. Instead, multiple classes can use the same shared logger instance.

Other examples:
- Application configuration manager
- Cache manager
- Shared resource manager

> Important: Singleton is useful when one shared instance is required. It should not be used everywhere because it can introduce shared state and make testing harder.

## 2. How to implement Singleton?

There are several ways to implement the Singleton pattern:

1. Eager Initialization
2. Lazy Initialization
3. Thread-Safe Singleton using synchronized
4. Double-Checked Locking
5. Bill Pugh Singleton
6. Enum Singleton

Let's understand each approach.

---

## 3. Eager Initialization

In eager initialization, the object is created when the class is loaded.

### Code

```java
class Singleton {

    private static final Singleton INSTANCE = new Singleton();

    private Singleton() {
        // Private constructor
    }

    public static Singleton getInstance() {
        return INSTANCE;
    }
}

public class Main {
    public static void main(String[] args) {

        Singleton obj1 = Singleton.getInstance();
        Singleton obj2 = Singleton.getInstance();

        System.out.println(obj1 == obj2);
    }
}
```

### Output

```text
true
```

### How does it work?

- The constructor is private, so other classes cannot create objects using `new`.
- The instance is created once when the class is initialized.
- The `getInstance()` method returns the same object every time.

### Advantages

- Simple implementation.
- Thread-safe because Java class initialization is thread-safe.
- No synchronization overhead when accessing the instance.

### Disadvantages

- The object is created even if nobody uses it.
- Not suitable if object creation is expensive and the instance may never be needed.

---

## 4. Lazy Initialization

In lazy initialization, the object is created only when someone requests it for the first time.

### Code

```java
class Singleton {

    private static Singleton instance;

    private Singleton() {
    }

    public static Singleton getInstance() {

        if (instance == null) {
            instance = new Singleton();
        }

        return instance;
    }
}
```

### How does it work?

- Initially, `instance` is null.
- When `getInstance()` is called for the first time, an object is created.
- Subsequent calls return the existing object.

### Advantages

- Object is created only when required.
- Easy to understand and implement.

### Disadvantages

- **Not thread-safe.**
- If two threads call `getInstance()` at the same time, both may see `instance == null` and create separate objects.

### Interview point

This approach is fine in a single-threaded environment, but it is not safe in a multithreaded application.

---

## 5. Thread-Safe Singleton Using synchronized

We can use the `synchronized` keyword to make lazy initialization thread-safe.

### Code

```java
class Singleton {

    private static Singleton instance;

    private Singleton() {
    }

    public static synchronized Singleton getInstance() {

        if (instance == null) {
            instance = new Singleton();
        }

        return instance;
    }
}
```

### How does it work?

- Only one thread can execute the synchronized method at a time.
- If two threads call `getInstance()` simultaneously, one thread enters the method first.
- It creates the object.
- The second thread enters later and receives the existing object.

### Advantages

- Thread-safe.
- Lazy initialization.
- Simple to implement.

### Disadvantages

- Every call to `getInstance()` requires acquiring the same monitor lock.
- This can add synchronization overhead, even after the object has been created.

---

## 6. Double-Checked Locking (DCL)

Double-Checked Locking is an optimized way to implement a thread-safe lazy Singleton.

It checks whether the instance is null twice:
1. Before acquiring the lock.
2. Again after acquiring the lock.

The second check is important because another thread might have created the object while the current thread was waiting for the lock.

### Code

```java
class Singleton {

    private static volatile Singleton instance;

    private Singleton() {
    }

    public static Singleton getInstance() {

        if (instance == null) {

            synchronized (Singleton.class) {

                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }

        return instance;
    }
}
```

### How does it work?

**First check:**

```java
if (instance == null)
```

If the object already exists, we return it without acquiring the lock.

**Acquire the lock:**

```java
synchronized (Singleton.class)
```

Only one thread at a time can enter this synchronized block.

**Second check:**

```java
if (instance == null)
```

We check again because another thread might have created the instance before this thread acquired the lock.

**Create the object:**

```java
instance = new Singleton();
```

The object is created only if it is still null.

### Why do we use volatile?

This is a very important interview question.

```java
private static volatile Singleton instance;
```

The `volatile` keyword ensures visibility between threads and prevents unsafe instruction reordering around publication of the instance.

Without `volatile`, another thread could observe a reference to an object before its constructor has completed.

### Advantages

- Thread-safe.
- Lazy initialization.
- Synchronization is needed only during the first creation.
- Subsequent calls avoid the synchronized block.

### Disadvantages

- More complex than other approaches.
- Easy to implement incorrectly if `volatile` or the second null check is missed.

### Interview point

Double-Checked Locking requires `volatile` in modern Java. Do not remove it.

---

## 7. Bill Pugh Singleton (Initialization-on-demand holder idiom)

This is one of the recommended approaches for a lazy, thread-safe Singleton.

It uses a static nested class to hold the Singleton instance.

### Code

```java
class Singleton {

    private Singleton() {
    }

    private static class SingletonHelper {

        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {

        return SingletonHelper.INSTANCE;
    }
}
```

### How does it work?

- Loading the outer `Singleton` class does not immediately create the instance.
- The nested `SingletonHelper` class is initialized only when `getInstance()` accesses it.
- Java guarantees thread-safe class initialization.
- Therefore, only one instance is created.

### Advantages

- Lazy initialization.
- Thread-safe without explicit synchronization.
- No need to use `volatile`.
- Clean and efficient implementation.

### Disadvantages

- Slightly less obvious to developers who are unfamiliar with nested classes and class initialization.

### Interview point

Bill Pugh Singleton is a good choice when you need lazy initialization with thread safety and want to avoid explicit synchronization.

---

## 8. Enum Singleton

Java also supports implementing Singleton using an enum.

### Code

```java
enum Singleton {

    INSTANCE;

    public void showMessage() {
        System.out.println("Singleton using Enum");
    }
}

public class Main {

    public static void main(String[] args) {

        Singleton obj1 = Singleton.INSTANCE;
        Singleton obj2 = Singleton.INSTANCE;

        obj1.showMessage();

        System.out.println(obj1 == obj2);
    }
}
```

### Output

```text
Singleton using Enum
true
```

### How does it work?

- Java creates one enum constant per enum type.
- We access that constant using `Singleton.INSTANCE`.
- Java handles thread-safe initialization.
- Enum serialization is handled safely by Java.
- Enum constants also resist ordinary reflection-based construction attacks.

### Advantages

- Simple implementation.
- Thread-safe.
- Serialization-safe by default.
- Strong protection against ordinary reflection-based creation of another instance.

### Disadvantages

- Cannot use lazy initialization in the same way as the Bill Pugh approach. The enum constant is initialized when the enum class is initialized.
- Less flexible if the class needs to extend another class, because enums already extend `java.lang.Enum`.

---

## 9. Comparison of Singleton Approaches

| Approach | Lazy | Thread-safe | Main point |
|---|---|---|---|
| Eager Initialization | No | Yes | Instance created during class initialization |
| Lazy Initialization | Yes | No | Simple, but unsafe with multiple threads |
| Synchronized Method | Yes | Yes | Locks every method call |
| Double-Checked Locking | Yes | Yes | Uses `volatile` and synchronized block |
| Bill Pugh | Yes | Yes | Uses static nested holder class |
| Enum | On enum class initialization | Yes | Simple and serialization-safe |

---

## 10. What is the difference between synchronized Singleton and Double-Checked Locking?

| Synchronized Singleton | Double-Checked Locking |
|---|---|
| Entire `getInstance()` method is synchronized | Only the creation block is synchronized |
| Lock is acquired on every call | Lock is acquired only when instance is not yet created |
| Easy to understand | More complex |
| Thread-safe | Thread-safe when implemented correctly with `volatile` |

---

## 11. Can Singleton be broken?

This is another common interview question.

Some Singleton implementations can be broken using reflection or serialization if they are not designed to handle these cases.

### Reflection

Reflection can access a private constructor and attempt to create another instance.

Example:

```java
Constructor<Singleton> constructor =
        Singleton.class.getDeclaredConstructor();

constructor.setAccessible(true);

Singleton obj = constructor.newInstance();
```

This can create another instance for many ordinary Singleton implementations.

**How can we prevent it?**

We can add a check inside the constructor:

```java
private Singleton() {

    if (instance != null) {
        throw new RuntimeException("Use getInstance()");
    }
}
```

This is a basic protection for common implementations, but it is not a universal defense against every attack or implementation detail.

Enum Singleton is generally the strongest simple choice against ordinary reflection-based construction.

### Serialization

When a normal Singleton class implements `Serializable`, deserialization can create another object unless we handle it.

One common solution is:

```java
protected Object readResolve() {
    return getInstance();
}
```

This tells Java serialization to return the existing Singleton instance after deserialization.

Enum Singleton handles serialization automatically.

---

## 12. Real-time Interview Scenario

**Question: You are developing a Spring Boot application. Where might you use Singleton?**

**Answer:**

A shared configuration or stateless service component can be a suitable example when one shared instance is required.

In Spring, beans are singleton-scoped by default. This means Spring creates one bean instance per bean definition per application context and shares it across injection points in that context.

Example:

```java
@Service
public class PaymentService {

    public void processPayment() {
        System.out.println("Processing payment");
    }
}
```

Spring manages the lifecycle of this service bean.

**Important:** Spring's singleton scope is not exactly the same as the classic Java Singleton pattern. It is one instance per Spring application context, not necessarily one instance per JVM.

Also, singleton-scoped Spring beans should generally be stateless or carefully designed for thread safety because multiple requests may use the same instance concurrently.

---

## 13. Common Interview Questions

**Q1. Why is the Singleton constructor private?**

To prevent other classes from creating objects directly using the `new` keyword.

**Q2. Is a basic lazy Singleton thread-safe?**

No. Multiple threads can create multiple instances if they access it at the same time.

**Q3. Why do we use volatile in Double-Checked Locking?**

To ensure visibility and prevent unsafe reordering when the instance is published to other threads.

**Q4. Which Singleton approach is best?**

- For simple eager creation: Eager Initialization.
- For lazy and thread-safe creation: Bill Pugh.
- For a simple serialization-safe Singleton: Enum Singleton.
- Double-Checked Locking is useful when you specifically need lazy initialization with a synchronized creation block.

**Q5. What is the difference between lazy and eager initialization?**

Eager initialization creates the instance during class initialization. Lazy initialization creates it only when it is first requested.

**Q6. Can we create a Singleton class using a private constructor and a static method?**

Yes. A private constructor prevents direct object creation, and the static method provides access to the shared instance. We must also handle thread safety if the application is multithreaded.

**Q7. Is Spring's default bean scope Singleton?**

Yes. Spring beans are singleton-scoped by default, meaning one instance per bean definition per application context.

---

## 14. Quick Revision

Remember these points before an interview:

- Singleton ensures one shared instance within its intended scope.
- Constructor should be private.
- Instance is usually stored in a static field.
- `getInstance()` provides access to the instance.
- Eager initialization creates the instance early.
- Lazy initialization creates it on first access.
- Synchronized method makes lazy initialization thread-safe.
- Double-Checked Locking uses two null checks, `synchronized`, and `volatile`.
- Bill Pugh uses a static nested holder class.
- Enum Singleton is simple and handles serialization safely.
- Spring beans are singleton-scoped by default.
