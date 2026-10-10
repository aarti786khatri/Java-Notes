# 🔷 Abstraction in Java

## 1. What is Abstraction?

**Abstraction means hiding internal implementation details and showing only the essential functionality to the user.**

It focuses on **what an object does rather than how it does it.**

In Java, abstraction is achieved using:
- **Abstract Classes**
- **Interfaces**

---

## 2. Abstract Class

An abstract class provides **0% to 100% abstraction**, depending on its implementation.

It can contain:
- Abstract methods (methods without a body)
- Concrete methods (methods with a body)
- Instance variables and constructors

### 💡 Real-Life Example: Bank Accounts

Imagine a bank has different types of accounts:
- Savings Account
- Current Account

Both accounts support common operations, such as:
- Deposit
- Withdrawal
- Calculate Interest

However, the **interest calculation may differ** for each account type.

We can create an abstract class called `BankAccount` that defines common functionality and declares `calculateInterest()` as an abstract method.

Each account provides its own implementation of the interest calculation.

### Java Example

```java
abstract class BankAccount {

    public void deposit(double amount) {
        System.out.println("Amount deposited: " + amount);
    }

    public abstract double calculateInterest();
}

class SavingsAccount extends BankAccount {

    @Override
    public double calculateInterest() {
        return 500.0;
    }
}

class CurrentAccount extends BankAccount {

    @Override
    public double calculateInterest() {
        return 0.0;
    }
}
```

**Key takeaway:** Use an abstract class when related classes share common state or implementation but need different implementations for certain operations.

---

## 3. Interface

An interface defines a **contract that implementing classes must follow**.

Traditionally, interfaces were described as providing 100% abstraction. However, modern Java interfaces can also contain `default`, `static`, and private methods with implementations.

### 💡 Real-Life Example: Payment at a Shop

Imagine you visit a shop and buy some items.

At the billing counter, the shopkeeper only needs to know whether the payment was successful.

The customer can pay using:
- UPI
- Debit Card
- Cash

The shopkeeper does not need to know the internal process of each payment method. The important thing is that each method follows the required payment contract.

An interface acts like a **rule at the billing counter**. It defines what must be done, but each payment method determines how it is done.

### Java Example

```java
interface Payment {
    void pay(double amount);
}

class UpiPayment implements Payment {

    @Override
    public void pay(double amount) {
        System.out.println("Paid using UPI: " + amount);
    }
}

class CardPayment implements Payment {

    @Override
    public void pay(double amount) {
        System.out.println("Paid using Debit Card: " + amount);
    }
}

class CashPayment implements Payment {

    @Override
    public void pay(double amount) {
        System.out.println("Paid using Cash: " + amount);
    }
}
```

### How Does It Work?

The billing system can work with any payment method that implements the `Payment` interface.

```java
class BillingCounter {

    public void checkout(Payment payment, double amount) {
        payment.pay(amount);
    }
}
```

Example usage:

```java
BillingCounter counter = new BillingCounter();

counter.checkout(new UpiPayment(), 1000);
counter.checkout(new CardPayment(), 2000);
counter.checkout(new CashPayment(), 500);
```

**Key takeaway:** The billing counter does not need to change its implementation for every new payment method. It depends on the `Payment` interface rather than a specific payment class.

---

## 4. Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---|---|---|
| Purpose | Share common state and implementation | Define a contract or capability |
| Methods | Abstract and concrete methods | Abstract methods and supported implemented methods |
| Variables | Instance and static variables | Implicitly `public static final` fields |
| Constructor | Allowed | Not allowed |
| Inheritance | A class can extend only one class | A class can implement multiple interfaces |
| Best suited for | Closely related classes | Different classes following the same contract |

---

## 5. How to Choose Between Them?

### Choose an Abstract Class When:
- Related classes share common state.
- You want to reuse common implementation.
- Some methods need different implementations in subclasses.

**Example:** `BankAccount` → `SavingsAccount`, `CurrentAccount`

### Choose an Interface When:
- You want to define a contract.
- Different classes should support the same capability.
- You want loose coupling and flexible implementations.

**Example:** `Payment` → `UpiPayment`, `CardPayment`, `CashPayment`

