# Encapsulation in Java

## 1. What is Encapsulation?

Encapsulation means **wrapping data and methods into a single class and restricting direct access to the data.**

In Java, we achieve encapsulation by declaring variables as `private` and providing public methods to access or modify them when required.

## 2. Why Do We Use Encapsulation?

- To protect data from direct access.
- To control how variables are modified.
- To apply validation before changing data.
- To make code easier to maintain.

## 3. Real-World Example

Consider a bank account with a balance of ₹10,000.

We should not allow anyone to change the balance directly. Instead, we provide methods such as `deposit()` to add money and `getBalance()` to check the balance.

This way, the class controls how its data is accessed and modified.

## 4. Java Example

```java
class BankAccount {

    private double balance = 10000;

    public void deposit(double amount) {
        if (amount > 0) {
            balance = balance + amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}

public class Main {
    public static void main(String[] args) {

        BankAccount account = new BankAccount();

        account.deposit(2000);

        System.out.println(account.getBalance());
    }
}
```

**Output:**

```text
12000.0
```

## 5. How Does It Work?

- `private double balance` prevents direct access to the variable from outside the `BankAccount` class.
- `deposit()` allows money to be added only when the amount is positive.
- `getBalance()` allows us to read the current balance.

If someone tries to access `account.balance` directly from `Main`, Java will give a compilation error because `balance` is private.

## 6. Important Points

- Encapsulation is commonly achieved using private variables and public methods.
- Getters and setters are not mandatory in every encapsulated class. We should expose only the operations that are actually needed.
- Encapsulation provides controlled access to data, rather than allowing unrestricted modification.

## 7. My Interview Understanding

Encapsulation means keeping data and the methods that operate on it inside a class while restricting direct access to the data.

For example, in a banking application, I would make the account balance private and provide methods to deposit money or check the balance. This allows the class to validate operations before modifying the balance.
