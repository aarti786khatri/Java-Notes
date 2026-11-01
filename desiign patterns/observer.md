# Observer Design Pattern in Java

## 1. What is the Observer Design Pattern?

Observer is a **behavioral design pattern** that defines a one-to-many relationship between objects.

When one object changes its state, all its registered dependent objects are automatically notified.

There are two main participants:

- **Subject:** The object that maintains a list of observers and sends notifications.
- **Observer:** The objects that register with the subject and receive notifications.

### Real-time example: YouTube subscription

Imagine a YouTube channel.

- A channel uploads a new video.
- Many users have subscribed to that channel.
- Whenever a new video is uploaded, all subscribers receive a notification.

Here:
- YouTube channel is the Subject.
- Subscribers are Observers.
- New video upload is the event that triggers notification.

## 2. Java Implementation

### Step 1: Create the Observer interface

```java
interface Observer {
    void update(String videoName);
}
```

Every subscriber will implement this interface.

### Step 2: Create the Subject interface

```java
interface Subject {

    void subscribe(Observer observer);

    void unsubscribe(Observer observer);

    void notifySubscribers(String videoName);
}
```

### Step 3: Create a Subscriber class

```java
class Subscriber implements Observer {

    private String name;

    public Subscriber(String name) {
        this.name = name;
    }

    @Override
    public void update(String videoName) {

        System.out.println(
            name + " received notification: New video uploaded - "
            + videoName
        );
    }
}
```

### Step 4: Create the YouTube Channel class

```java
import java.util.ArrayList;
import java.util.List;

class YouTubeChannel implements Subject {

    private List<Observer> subscribers = new ArrayList<>();

    @Override
    public void subscribe(Observer observer) {
        subscribers.add(observer);
    }

    @Override
    public void unsubscribe(Observer observer) {
        subscribers.remove(observer);
    }

    @Override
    public void notifySubscribers(String videoName) {

        for (Observer observer : subscribers) {
            observer.update(videoName);
        }
    }

    public void uploadVideo(String videoName) {

        System.out.println("New video uploaded: " + videoName);

        notifySubscribers(videoName);
    }
}
```

### Step 5: Main class

```java
public class Main {

    public static void main(String[] args) {

        YouTubeChannel channel = new YouTubeChannel();

        Observer user1 = new Subscriber("Aarti");
        Observer user2 = new Subscriber("Rahul");
        Observer user3 = new Subscriber("Priya");

        // Subscribe users
        channel.subscribe(user1);
        channel.subscribe(user2);
        channel.subscribe(user3);

        // Upload a new video
        channel.uploadVideo("Java Design Patterns");

        // Unsubscribe one user
        channel.unsubscribe(user2);

        System.out.println("After Rahul unsubscribes:");

        // Upload another video
        channel.uploadVideo("Spring Boot Interview Questions");
    }
}
```

### Output

```text
New video uploaded: Java Design Patterns
Aarti received notification: New video uploaded - Java Design Patterns
Rahul received notification: New video uploaded - Java Design Patterns
Priya received notification: New video uploaded - Java Design Patterns

After Rahul unsubscribes:

New video uploaded: Spring Boot Interview Questions
Aarti received notification: New video uploaded - Spring Boot Interview Questions
Priya received notification: New video uploaded - Spring Boot Interview Questions
```

## 3. How does it work?

1. The YouTube channel maintains a list of subscribers.
2. Users subscribe by calling `subscribe()`.
3. When a new video is uploaded, `uploadVideo()` is called.
4. The channel calls `notifySubscribers()`.
5. Every registered observer receives the update.
6. If a user unsubscribes, that user no longer receives notifications.

### Flow

```text
             YouTube Channel
                 (Subject)
                     |
              Upload New Video
                     |
                     v
             notifySubscribers()
                     |
          -------------------------
          |           |           |
          v           v           v
       Aarti        Rahul       Priya
      Observer    Observer     Observer
          |           |           |
          v           v           v
      Notification Notification Notification
```

## 4. Why do we use Observer?

Imagine that we have 1,000 subscribers.

Without Observer, the channel would need to know about every subscriber and manage notification logic directly.

With Observer:
- Subscribers register themselves.
- The subject maintains the observer list.
- The subject notifies all registered observers when an event occurs.
- New observers can be added without changing the subject's notification logic.

## 5. Advantages

- Reduces tight coupling between the subject and observers.
- Supports one-to-many communication.
- Observers can subscribe and unsubscribe dynamically.
- New observers can be added without changing the subject's core logic.

## 6. Disadvantages

- If many observers are registered, notification processing can become expensive.
- The order of notifications may not always be guaranteed unless explicitly managed.
- Care is needed to avoid memory leaks when observers are not unsubscribed.
- Synchronous notification can make the subject wait for slow observers.

## 7. Observer in Java and Spring Boot

### Java

Java provides `PropertyChangeSupport` for property-change notifications. The older `java.util.Observer` and `Observable` APIs are deprecated and should generally be avoided in new code.

### Spring Boot

Spring supports event-based communication using:
- `ApplicationEvent`
- `ApplicationEventPublisher`
- `@EventListener`

Example:

Publishing an event:

```java
@Service
class OrderService {

    private final ApplicationEventPublisher publisher;

    OrderService(ApplicationEventPublisher publisher) {
        this.publisher = publisher;
    }

    public void placeOrder() {

        System.out.println("Order placed successfully");

        publisher.publishEvent(
            new OrderPlacedEvent(this, "ORDER-101")
        );
    }
}
```

Event class:

```java
class OrderPlacedEvent {

    private final Object source;
    private final String orderId;

    public OrderPlacedEvent(Object source, String orderId) {
        this.source = source;
        this.orderId = orderId;
    }

    public Object getSource() {
        return source;
    }

    public String getOrderId() {
        return orderId;
    }
}
```

Listener:

```java
@Component
class NotificationListener {

    @EventListener
    public void handleOrderPlaced(OrderPlacedEvent event) {

        System.out.println(
            "Sending notification for order: " + event.getOrderId()
        );
    }
}
```

When the order service publishes the event, Spring calls the registered event listener.

This is a useful way to separate order processing from notification handling.

Note: Spring application events are normally handled synchronously by default. Asynchronous handling can be configured separately.


## 9. Quick Revision

- Observer is a behavioral design pattern.
- It defines a one-to-many relationship.
- Subject maintains observers.
- Observers receive updates when an event occurs.
- Users can subscribe and unsubscribe.
- YouTube subscription is a simple example.
- Spring Boot supports event-based communication using `ApplicationEventPublisher` and `@EventListener`.
