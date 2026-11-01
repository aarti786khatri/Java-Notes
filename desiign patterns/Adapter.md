# Adapter Design Pattern in Java

## 1. What is the Adapter Design Pattern?

Adapter is a **structural design pattern** that allows two incompatible interfaces to work together.

It acts as a bridge between the client and an existing class whose interface does not match what the client expects.

### Real-time example: Payment system

Imagine our application expects every payment provider to follow this interface:

```java
interface PaymentProcessor {
    void pay(double amount);
}
```

But an existing third-party payment service has a different method:

```java
class OldPaymentGateway {

    public void makePayment(double amount) {
        System.out.println("Payment completed: " + amount);
    }
}
```

We cannot directly use `OldPaymentGateway` where `PaymentProcessor` is expected because the method names are different.

We can create an Adapter.

## 2. Java Implementation

### Step 1: Target interface

```java
interface PaymentProcessor {
    void pay(double amount);
}
```

### Step 2: Existing third-party class

```java
class OldPaymentGateway {

    public void makePayment(double amount) {
        System.out.println("Payment completed: " + amount);
    }
}
```

### Step 3: Create the Adapter

```java
class PaymentAdapter implements PaymentProcessor {

    private OldPaymentGateway gateway;

    public PaymentAdapter(OldPaymentGateway gateway) {
        this.gateway = gateway;
    }

    @Override
    public void pay(double amount) {
        gateway.makePayment(amount);
    }
}
```

### Step 4: Use the Adapter

```java
public class Main {

    public static void main(String[] args) {

        OldPaymentGateway oldGateway = new OldPaymentGateway();

        PaymentProcessor processor =
                new PaymentAdapter(oldGateway);

        processor.pay(500);
    }
}
```

### Output

```text
Payment completed: 500.0
```

## 3. How does it work?

1. The application expects a `PaymentProcessor`.
2. The old gateway provides `makePayment()` instead of `pay()`.
3. `PaymentAdapter` implements the expected interface.
4. It internally calls the old gateway's method.
5. The application can now use the old gateway through the new interface.

### Flow

```text
        Main Application
               |
               v
       PaymentProcessor
           Interface
               |
               v
        PaymentAdapter
               |
               v
       OldPaymentGateway
          makePayment()
```

## 4. Advantages

- Allows incompatible interfaces to work together.
- Helps reuse existing or third-party code.
- Avoids changing legacy classes.
- Keeps integration logic separate from business logic.

## 5. Disadvantages

- Adds another class to the application.
- Too many adapters can make the design harder to understand.

## 6. Common Interview Questions

**Q1. What is the Adapter Design Pattern?**

It is a structural design pattern that allows incompatible interfaces to work together.

**Q2. Give a real-time example.**

A payment adapter can convert the application's expected `pay()` call into a third-party gateway's `makePayment()` call.

**Q3. Why do we use Adapter?**

To integrate existing or third-party classes without changing their original implementation.

**Q4. Is Adapter a creational or structural pattern?**

It is a structural design pattern.

## 7. Quick Revision

- Adapter connects incompatible interfaces.
- It acts as a bridge between two systems.
- It is useful for third-party integrations and legacy systems.
- In this example, the adapter converts `pay()` into `makePayment()`.

### Interview-ready answer

"Adapter is a structural design pattern that helps incompatible interfaces work together. For example, if our application expects a PaymentProcessor with a `pay()` method but an existing payment gateway provides `makePayment()`, we create an adapter that implements PaymentProcessor and internally calls the gateway method."
