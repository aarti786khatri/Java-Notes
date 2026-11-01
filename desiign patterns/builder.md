# Builder Design Pattern in Java

## 1. What is the Builder Design Pattern?

Builder is a **creational design pattern** used to create complex objects step by step.

It is especially useful when:
- A class has many fields.
- Some fields are mandatory and others are optional.
- We want readable object creation code.
- We want to avoid constructors with many parameters.

### Real-time example: Game Character

Imagine a game where a character has:
- Name
- Health
- Level
- Weapon
- Armor
- Special power

Some values are required, while others are optional.

Without Builder, we may need many constructors.

```java
GameCharacter character = new GameCharacter(
    "Warrior", 100, 10, "Sword", "Shield", "Fire"
);
```

It can become difficult to remember the order of parameters.

Builder makes this easier.

## 2. Java Implementation

### Step 1: Create the GameCharacter class

```java
class GameCharacter {

    private final String name;
    private final int health;
    private final int level;
    private final String weapon;
    private final String armor;

    private GameCharacter(Builder builder) {
        this.name = builder.name;
        this.health = builder.health;
        this.level = builder.level;
        this.weapon = builder.weapon;
        this.armor = builder.armor;
    }

    public void display() {
        System.out.println("Name: " + name);
        System.out.println("Health: " + health);
        System.out.println("Level: " + level);
        System.out.println("Weapon: " + weapon);
        System.out.println("Armor: " + armor);
    }

    public static class Builder {

        private final String name;
        private final int health;

        private int level;
        private String weapon;
        private String armor;

        public Builder(String name, int health) {
            this.name = name;
            this.health = health;
        }

        public Builder level(int level) {
            this.level = level;
            return this;
        }

        public Builder weapon(String weapon) {
            this.weapon = weapon;
            return this;
        }

        public Builder armor(String armor) {
            this.armor = armor;
            return this;
        }

        public GameCharacter build() {
            return new GameCharacter(this);
        }
    }
}
```

### Step 2: Create the object

```java
public class Main {

    public static void main(String[] args) {

        GameCharacter character =
                new GameCharacter.Builder("Warrior", 100)
                        .level(10)
                        .weapon("Sword")
                        .armor("Shield")
                        .build();

        character.display();
    }
}
```

### Output

```text
Name: Warrior
Health: 100
Level: 10
Weapon: Sword
Armor: Shield
```

## 3. How does Builder work?

1. We create a `Builder` object.
2. We pass mandatory values through the Builder constructor.
3. We set optional values using methods such as `weapon()` and `armor()`.
4. Each method returns `this`, allowing method chaining.
5. Finally, `build()` creates and returns the actual `GameCharacter` object.

### Flow

```text
       Builder Constructor
          (name, health)
                |
                v
          .level(10)
                |
                v
         .weapon("Sword")
                |
                v
         .armor("Shield")
                |
                v
             .build()
                |
                v
         GameCharacter
```

## 4. Why does each Builder method return this?

```java
public Builder weapon(String weapon) {
    this.weapon = weapon;
    return this;
}
```

`this` refers to the current Builder object.

Returning it allows us to call multiple methods in one statement. This is called **method chaining**.

## 5. Advantages

- Makes object creation readable.
- Avoids constructors with many parameters.
- Supports optional fields.
- Helps create immutable objects when fields are final and there are no setters.
- Makes validation easier inside the `build()` method.

## 6. Disadvantages

- Requires additional code and a Builder class.
- Can be unnecessary for simple objects with only two or three fields.

## 7. Builder vs Constructor

| Constructor | Builder |
|---|---|
| Creates an object directly | Creates an object step by step |
| Many parameters can be confusing | Methods have meaningful names |
| Optional values can require many overloaded constructors | Optional values can be set individually |
| Simpler for small objects | Useful for objects with many fields |

## 8. Common Interview Questions

**Q1. What is the Builder Design Pattern?**

It is a creational design pattern that creates complex objects step by step.

**Q2. Why do we use Builder?**

To make object creation readable when a class has many mandatory and optional fields.

**Q3. What is method chaining?**

Calling multiple methods on the same object in a single statement. Builder methods commonly return `this` to support it.

**Q4. Where do we use Builder in Java?**

For example, `StringBuilder` uses a builder-style API. Libraries such as Lombok also provide `@Builder` to generate Builder code.

**Q5. How can Builder help create immutable objects?**

We can keep fields final, use a private constructor, and expose no setters. The Builder sets the values before creating the final object.

## 9. Quick Revision

- Builder is a creational design pattern.
- It creates complex objects step by step.
- It is useful when a class has many fields.
- It avoids telescoping constructors.
- `return this` supports method chaining.
- `build()` creates the final object.

### Interview-ready answer

"Builder is a creational design pattern used to create complex objects step by step. For example, a game character may have name, health, level, weapon and armor. Some fields are mandatory and others are optional. Instead of creating multiple constructors, we use a Builder class and set values through chained methods. Finally, the build method creates the object."
