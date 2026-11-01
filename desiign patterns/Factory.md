# Factory Design Pattern and Abstract Factory in Java

## 1. What is the Factory Design Pattern?

Factory is a creational design pattern used to create objects without exposing the object creation logic to the client.

Instead of creating objects directly using `new` in multiple places, we keep the object creation logic inside a factory class.

### Real-time example: Notification system

Imagine we are developing an application that sends notifications.

The application supports:
- Email notification
- SMS notification
- Push notification

Depending on the user's choice, we need to create the correct notification object.

Without Factory, we might write:

```java
if (type.equals("EMAIL")) {
    notification = new EmailNotification();
} else if (type.equals("SMS")) {
    notification = new SmsNotification();
} else if (type.equals("PUSH")) {
    notification = new PushNotification();
}
```

If this object creation logic is repeated in multiple places, maintaining the code becomes difficult.

We can move this logic into a Factory class.

## 2. Factory Method Example: Notification Factory

### Step 1: Create a common interface

```java
interface Notification {
    void send(String message);
}
```

All notification classes will implement this interface.

### Step 2: Create implementation classes

```java
class EmailNotification implements Notification {

    @Override
    public void send(String message) {
        System.out.println("Email sent: " + message);
    }
}
```

```java
class SmsNotification implements Notification {

    @Override
    public void send(String message) {
        System.out.println("SMS sent: " + message);
    }
}
```

```java
class PushNotification implements Notification {

    @Override
    public void send(String message) {
        System.out.println("Push notification sent: " + message);
    }
}
```

### Step 3: Create the Factory class

```java
class NotificationFactory {

    public static Notification createNotification(String type) {

        if (type == null) {
            throw new IllegalArgumentException("Notification type cannot be null");
        }

        switch (type.toUpperCase()) {

            case "EMAIL":
                return new EmailNotification();

            case "SMS":
                return new SmsNotification();

            case "PUSH":
                return new PushNotification();

            default:
                throw new IllegalArgumentException(
                    "Invalid notification type: " + type
                );
        }
    }
}
```

### Step 4: Use the Factory in the Main class

```java
public class Main {

    public static void main(String[] args) {

        Notification notification =
                NotificationFactory.createNotification("EMAIL");

        notification.send("Your order has been placed successfully!");

        Notification sms =
                NotificationFactory.createNotification("SMS");

        sms.send("Your OTP is 123456");
    }
}
```

### Output

```text
Email sent: Your order has been placed successfully!
SMS sent: Your OTP is 123456
```

### How does it work?

1. The client asks `NotificationFactory` to create a notification.
2. The factory checks the requested notification type.
3. It creates and returns the appropriate implementation.
4. The client calls `send()` without knowing the concrete class being used.

### Flow

```text
             Main Class
                 |
                 |
       NotificationFactory
                 |
        -------------------
        |        |        |
      Email     SMS      Push
        |        |        |
        -------------------
                 |
          Notification
             Interface
```

### Advantages

- Object creation logic is kept in one place.
- Client code does not need to know the concrete implementation.
- Adding another notification type is easier.
- Helps reduce direct dependencies on implementation classes.

### Disadvantages

- The factory class can become large when many types are added.
- A simple factory using `if`, `switch` or `else` is not automatically the formal Factory Method pattern from GoF.

## 3. Factory Method vs Simple Factory

These terms are often used interchangeably in everyday Java discussions, but there is a distinction.

**Simple Factory:** One factory class decides which concrete object to create using conditions such as `if` or `switch`.

**Factory Method:** A creator class defines a method for creating an object, and subclasses decide which concrete product to return.

The notification example above is a Simple Factory.

Here is a small Factory Method example:

```java
interface Notification {
    void send(String message);
}

class EmailNotification implements Notification {
    public void send(String message) {
        System.out.println("Email: " + message);
    }
}

class SmsNotification implements Notification {
    public void send(String message) {
        System.out.println("SMS: " + message);
    }
}

abstract class NotificationCreator {

    public abstract Notification createNotification();

    public void notifyUser(String message) {
        Notification notification = createNotification();
        notification.send(message);
    }
}

class EmailNotificationCreator extends NotificationCreator {

    @Override
    public Notification createNotification() {
        return new EmailNotification();
    }
}

class SmsNotificationCreator extends NotificationCreator {

    @Override
    public Notification createNotification() {
        return new SmsNotification();
    }
}
```

Usage:

```java
public class Main {

    public static void main(String[] args) {

        NotificationCreator creator =
                new EmailNotificationCreator();

        creator.notifyUser("Welcome to our application!");
    }
}
```

Here, the subclasses decide which notification object to create by overriding `createNotification()`.

---

## 4. What is Abstract Factory Design Pattern?

Abstract Factory is also a creational design pattern.

It provides an interface for creating **families of related objects** without specifying their concrete classes.

### Real-time example: Notification and templates

Imagine our application supports two notification channels:

- Email
- SMS

Each channel has its own notification implementation and matching message template.

For example:

| Email family | SMS family |
|---|---|
| EmailNotification | SmsNotification |
| EmailTemplate | SmsTemplate |

We want to ensure that when the application selects Email, it receives both an Email notification and an Email template.

Similarly, when it selects SMS, it receives both SMS-related objects.

This is a good use case for Abstract Factory.

### Step 1: Create the product interfaces

```java
interface Notification {
    void send(String message);
}

interface MessageTemplate {
    String formatMessage(String message);
}
```

### Step 2: Create Email implementations

```java
class EmailNotification implements Notification {

    public void send(String message) {
        System.out.println("Email sent: " + message);
    }
}

class EmailTemplate implements MessageTemplate {

    public String formatMessage(String message) {
        return "Dear User, " + message;
    }
}
```

### Step 3: Create SMS implementations

```java
class SmsNotification implements Notification {

    public void send(String message) {
        System.out.println("SMS sent: " + message);
    }
}

class SmsTemplate implements MessageTemplate {

    public String formatMessage(String message) {
        return "SMS: " + message;
    }
}
```

### Step 4: Create the Abstract Factory interface

```java
interface NotificationFactory {

    Notification createNotification();

    MessageTemplate createTemplate();
}
```

The factory provides methods to create both related objects.

### Step 5: Create concrete factories

Email factory:

```java
class EmailFactory implements NotificationFactory {

    public Notification createNotification() {
        return new EmailNotification();
    }

    public MessageTemplate createTemplate() {
        return new EmailTemplate();
    }
}
```

SMS factory:

```java
class SmsFactory implements NotificationFactory {

    public Notification createNotification() {
        return new SmsNotification();
    }

    public MessageTemplate createTemplate() {
        return new SmsTemplate();
    }
}
```

### Step 6: Use the Abstract Factory

```java
public class Main {

    public static void main(String[] args) {

        NotificationFactory factory = new EmailFactory();

        Notification notification = factory.createNotification();

        MessageTemplate template = factory.createTemplate();

        String message = template.formatMessage(
                "Your order has been placed successfully!"
        );

        notification.send(message);
    }
}
```

### Output

```text
Email sent: Dear User, Your order has been placed successfully!
```

### How does it work?

1. The client selects an `EmailFactory`.
2. The factory creates an `EmailNotification`.
3. It also creates an `EmailTemplate`.
4. The client uses the matching objects without knowing their concrete implementation details.

### Flow

```text
              Main Class
                  |
                  |
       NotificationFactory
          (Interface)
                  |
          -----------------
          |               |
      EmailFactory     SmsFactory
          |               |
     -------------    -------------
     |           |    |           |
   Email       Email  SMS         SMS
Notification Template Notification Template
```

## 5. Factory vs Abstract Factory

| Factory Method / Simple Factory | Abstract Factory |
|---|---|
| Creates a product object | Creates a family of related objects |
| Focuses on one product type | Focuses on multiple related product types |
| Example: Email or SMS notification | Example: Email notification + Email template |
| Usually has one creation method per product | Has multiple creation methods for related products |

## 6. Common Interview Questions

**Q1. What is the Factory Design Pattern?**

It is a creational design pattern that centralizes object creation and hides the concrete implementation from the client.

**Q2. Why do we use Factory?**

We use it to avoid repeating object creation logic and to reduce direct dependency on concrete classes.

**Q3. What is Abstract Factory?**

It provides an interface to create families of related objects without exposing their concrete classes.

**Q4. What is the difference between Factory and Abstract Factory?**

Factory focuses on creating a product, while Abstract Factory focuses on creating a family of related products.

**Q5. Can we use Factory in a Spring Boot application?**

Yes. For example, a notification factory can select Email, SMS or Push notification implementations based on the notification type. Spring can also inject the available implementations into the factory.

## 7. Quick Revision

- Factory is a creational design pattern.
- It centralizes object creation.
- Simple Factory selects an implementation using conditions.
- Factory Method lets subclasses decide which product to create.
- Abstract Factory creates families of related objects.
- Notification Factory is a good example of Factory.
- Email notification + Email template is a good example of Abstract Factory.

### Interview-ready answer

"Factory Design Pattern helps us create objects without exposing the creation logic to the client. For example, in a notification system, we can have a NotificationFactory that creates Email, SMS or Push notification objects based on the requested type.

Abstract Factory goes one step further. It creates families of related objects. For example, an EmailFactory can create both an EmailNotification and an EmailTemplate, while an SmsFactory creates an SmsNotification and an SmsTemplate."
