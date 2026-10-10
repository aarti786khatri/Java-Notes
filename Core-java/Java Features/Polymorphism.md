
# Polymorphism in Java

Polymorphism means **"many forms."**

It allows the same method name or reference to behave differently depending on the situation.

In Java, polymorphism is mainly achieved through:

1. **Method Overloading**, which is compile-time polymorphism.
2. **Method Overriding**, which is runtime polymorphism.

---

## 1. Compile-Time Polymorphism (Method Overloading)

Compile-time polymorphism happens when multiple methods in the same class have the same name but different parameter lists.

The parameters can differ in:
- Number of arguments
- Types of arguments
- Order of argument types

The compiler decides which method to call based on the arguments passed to it. This decision is made at compile time.

### Example: Calculator

A calculator can perform addition on two integers, three integers, or two decimal numbers. We can use the same method name, `add()`, for all three operations.

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }

    double add(double a, double b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {

        Calculator calculator = new Calculator();

        System.out.println(calculator.add(10, 20));
        System.out.println(calculator.add(10, 20, 30));
        System.out.println(calculator.add(10.5, 20.5));
    }
}
```

**Output:**

```text
30
60
31.0
```

### How does it work?

```text
                 add()
                   |
        +----------+----------+
        |          |          |
    add(int,   add(int,    add(double,
       int)    int, int)   double)
        |          |          |
      30           60        31.0
```

The method name is the same, but the parameter lists are different. The compiler selects the matching method based on the arguments.

**Important:** Changing only the return type is not enough to overload a method.

For example, these two methods cannot exist together in the same class:

```java
int calculate(int a) {
    return a;
}

// Compilation error: same parameter list
double calculate(int a) {
    return a;
}
```

---

## 2. Runtime Polymorphism (Method Overriding)

Runtime polymorphism happens when a child class provides its own implementation of a method already defined in its parent class.

The method name and parameter list must match the overridden method, subject to Java's overriding rules.

When a parent class reference points to a child class object, Java decides at runtime which overridden method to execute. The actual object type determines the implementation.

### Example: Payment Processing

Imagine a banking application that supports different payment methods, such as credit card payments and UPI payments.

Both payment methods perform a payment, but their implementations are different.

```java
class Payment {

    void pay() {
        System.out.println("Processing payment");
    }
}

class CreditCardPayment extends Payment {

    @Override
    void pay() {
        System.out.println("Processing credit card payment");
    }
}

class UpiPayment extends Payment {

    @Override
    void pay() {
        System.out.println("Processing UPI payment");
    }
}

public class Main {
    public static void main(String[] args) {

        Payment payment;

        payment = new CreditCardPayment();
        payment.pay();

        payment = new UpiPayment();
        payment.pay();
    }
}
```

**Output:**

```text
Processing credit card payment
Processing UPI payment
```

### How does it work?

```text
                   Payment
                      |
                    pay()
                      |
             +--------+--------+
             |                 |
    CreditCardPayment      UpiPayment
             |                 |
           pay()              pay()
             |                 |
    Credit card payment   UPI payment
```

Here, the reference variable `payment` has the parent type `Payment`, but it points to different child objects.

- When it points to `CreditCardPayment`, the credit card implementation runs.
- When it points to `UpiPayment`, the UPI implementation runs.

Java selects the overridden method at runtime based on the actual object.

---

## 3. Five Real-World Examples of Compile-Time Polymorphism

| Example | Method overloading |
|---|---|
| Calculator | `add(int, int)` and `add(double, double)` |
| Logging | `log(String)` and `log(String, Throwable)` |
| Searching | `search(int)` and `search(String)` |
| Notifications | `send(String)` and `send(String, String)` |
| Report generation | `generateReport(int)` and `generateReport(int, String)` |

These are examples of method designs. Each pair can be implemented as overloaded methods in the same class.

### Example: Notification Service

```java
class NotificationService {

    void send(String message) {
        System.out.println("Message: " + message);
    }

    void send(String message, String recipient) {
        System.out.println(
            "Sending to " + recipient + ": " + message
        );
    }
}
```

The same method name, `send()`, supports different parameter lists.

---

## 4. Five Real-World Examples of Runtime Polymorphism

| Example | Parent type | Different implementations |
|---|---|---|
| Payment processing | `Payment` | Credit card, UPI |
| Notifications | `Notification` | Email, SMS |
| File processing | `FileProcessor` | PDF, CSV |
| Database access | `Database` | MySQL, PostgreSQL |
| Report generation | `Report` | PDF report, Excel report |

### Example: Notification Service

```java
class Notification {

    void send() {
        System.out.println("Sending notification");
    }
}

class EmailNotification extends Notification {

    @Override
    void send() {
        System.out.println("Sending email");
    }
}

class SmsNotification extends Notification {

    @Override
    void send() {
        System.out.println("Sending SMS");
    }
}

public class Main {
    public static void main(String[] args) {

        Notification notification;

        notification = new EmailNotification();
        notification.send();

        notification = new SmsNotification();
        notification.send();
    }
}
```

**Output:**

```text
Sending email
Sending SMS
```

This approach lets us add new notification types without changing the code that calls `send()`, as long as the caller works with the common `Notification` type.

---

## 5. Compile-Time vs Runtime Polymorphism

| Feature | Compile-Time | Runtime |
|---|---|---|
| Achieved through | Method overloading | Method overriding |
| Relationship | Same class is common, though overloading can also involve inheritance | Parent-child inheritance |
| Parameters | Must differ | Must match, subject to overriding rules |
| Method selection | Compiler selects an overload | JVM dispatches to the overridden implementation |
| Depends on | Argument types and method signatures | Actual object type |
| Inheritance required? | No | Yes, for class-based overriding |
| Example | `add(10, 20)` | `payment.pay()` |

## 6. My Understanding

I understand polymorphism as the ability to represent different behaviors through the same method name or common reference.

In method overloading, the compiler selects the appropriate method based on the parameter list. In method overriding, a child class provides its own implementation, and Java selects that implementation at runtime based on the actual object.

In a Spring Boot application, runtime polymorphism is useful when different implementations of an interface provide the same operation. For example, a payment service can have separate implementations for UPI and credit card payments.
