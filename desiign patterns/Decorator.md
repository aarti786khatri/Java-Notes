# Decorator Design Pattern in Java

## 1. What is the Decorator Design Pattern?

Decorator is a **structural design pattern** used to add additional behavior or responsibilities to an existing object without modifying its original class.

It wraps an existing object inside another object and adds extra functionality.

### Real-time example: Coffee ordering system

Imagine we have a coffee shop.

A customer can order:
- Simple coffee
- Coffee with milk
- Coffee with sugar
- Coffee with milk and sugar

Instead of creating separate classes for every combination, we can use Decorator.

For example:
- Start with simple coffee.
- Add milk.
- Add sugar.

Each decorator adds something to the original coffee.

## 2. Java Implementation

### Step 1: Create a common interface

```java
interface Coffee {
    String getDescription();
    double getCost();
}
```

### Step 2: Create the basic coffee

```java
class SimpleCoffee implements Coffee {

    public String getDescription() {
        return "Simple Coffee";
    }

    public double getCost() {
        return 50;
    }
}
```

### Step 3: Create an abstract decorator

```java
abstract class CoffeeDecorator implements Coffee {

    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
}
```

The decorator holds a reference to another Coffee object.

### Step 4: Create Milk Decorator

```java
class MilkDecorator extends CoffeeDecorator {

    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    public String getDescription() {
        return coffee.getDescription() + " + Milk";
    }

    public double getCost() {
        return coffee.getCost() + 20;
    }
}
```

### Step 5: Create Sugar Decorator

```java
class SugarDecorator extends CoffeeDecorator {

    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    public String getDescription() {
        return coffee.getDescription() + " + Sugar";
    }

    public double getCost() {
        return coffee.getCost() + 10;
    }
}
```

### Step 6: Main class

```java
public class Main {

    public static void main(String[] args) {

        Coffee coffee = new SimpleCoffee();

        // Add milk
        coffee = new MilkDecorator(coffee);

        // Add sugar
        coffee = new SugarDecorator(coffee);

        System.out.println(coffee.getDescription());
        System.out.println("Total cost: " + coffee.getCost());
    }
}
```

### Output

```text
Simple Coffee + Milk + Sugar
Total cost: 80.0
```

## 3. How does it work?

1. We create a `SimpleCoffee` object.
2. We wrap it inside `MilkDecorator`.
3. We wrap the result inside `SugarDecorator`.
4. Each decorator adds its own description and cost.
5. The final object contains all the added behavior.

### Flow

```text
       SimpleCoffee
            |
            v
      MilkDecorator
            |
            v
      SugarDecorator
            |
            v
        Final Coffee

       Cost = 50 + 20 + 10
             = 80
```

## 4. Why do we use Decorator?

Suppose we create separate classes for every coffee combination:

- SimpleCoffee
- MilkCoffee
- SugarCoffee
- MilkSugarCoffee
- MilkSugarChocolateCoffee

As the number of combinations increases, the number of classes also increases.

Decorator allows us to add features independently and combine them as required.

## 5. Advantages

- Adds functionality without changing the original class.
- Supports flexible combinations of features.
- Follows the Open/Closed Principle.
- Avoids creating a separate subclass for every combination.

## 6. Disadvantages

- Creates multiple wrapper objects.
- The execution flow can become difficult to understand when decorators are deeply nested.
- The order of decorators can affect the final behavior.

## 7. Decorator in Java

A common example is Java I/O streams.

```java
FileInputStream file =
        new FileInputStream("data.txt");

BufferedInputStream buffered =
        new BufferedInputStream(file);

DataInputStream data =
        new DataInputStream(buffered);
```

Each wrapper adds additional functionality around the previous stream.

## 8. Common Interview Questions

**Q1. What is the Decorator Design Pattern?**

It is a structural design pattern that adds additional behavior to an object without modifying its original class.

**Q2. Give a real-time example.**

In a coffee shop, we can start with simple coffee and add milk or sugar using decorators.

**Q3. Why do we use Decorator instead of inheritance?**

Decorator allows us to combine features dynamically at runtime. Inheritance can require many subclasses for different combinations.

**Q4. Where is Decorator used in Java?**

Java I/O streams are a common example, such as `BufferedInputStream` wrapping `FileInputStream`.

**Q5. Which principle does Decorator support?**

The Open/Closed Principle. We can add new behavior without modifying the existing class.

## 9. Quick Revision

- Decorator is a structural design pattern.
- It adds behavior to an existing object.
- It uses composition and object wrapping.
- Multiple decorators can be combined.
- Java I/O streams are a common example.

### Interview-ready answer

"Decorator is a structural design pattern used to add additional functionality to an object without modifying its original class. For example, in a coffee application, we can start with simple coffee and wrap it with MilkDecorator and SugarDecorator. Each decorator adds its own cost and description. This allows us to add features dynamically without creating separate classes for every combination."
