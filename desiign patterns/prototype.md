# Prototype Design Pattern in Java

## 1. What is the Prototype Design Pattern?

Prototype is a **creational design pattern** used to create new objects by copying an existing object.

Instead of creating an object from scratch and assigning all its properties again, we create a copy of an existing object.

The existing object is called the **Prototype**.

### Real-time Game Example

Imagine we are developing a multiplayer game.

The game has different characters, such as:
- Warrior
- Archer
- Wizard

Each character has properties like:
- Name
- Health
- Level
- Weapon
- Power

Suppose we already have a Warrior character with its properties configured.

Now, we want to create 100 Warrior characters with the same initial configuration.

Instead of creating each character from scratch and assigning all properties again, we can clone the existing Warrior object.

Each cloned character gets its own object, so changing one character's health does not affect the others.

This is where the Prototype Design Pattern is useful.

---

## 2. How does Prototype work?

The basic flow is:

```text
        Existing Game Character
             (Prototype)
                  |
                  |
             clone()
                  |
          -----------------
          |       |       |
       Warrior1 Warrior2 Warrior3
```

Each new character is created by copying the prototype.

---

## 3. Java Implementation Using Cloneable

Java provides the `Cloneable` interface and `Object.clone()` method for a basic cloning implementation.

Let's create a game character example.

### Step 1: Create the GameCharacter class

```java
class GameCharacter implements Cloneable {

    private String name;
    private int health;
    private int level;
    private String weapon;

    public GameCharacter(String name, int health,
                         int level, String weapon) {
        this.name = name;
        this.health = health;
        this.level = level;
        this.weapon = weapon;
    }

    public void setHealth(int health) {
        this.health = health;
    }

    public void setLevel(int level) {
        this.level = level;
    }

    @Override
    public GameCharacter clone() {

        try {
            return (GameCharacter) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError(e);
        }
    }

    public void displayCharacter() {

        System.out.println("Name: " + name);
        System.out.println("Health: " + health);
        System.out.println("Level: " + level);
        System.out.println("Weapon: " + weapon);
        System.out.println("------------------");
    }
}
```

### Step 2: Create the Main class

```java
public class Main {

    public static void main(String[] args) {

        // Create the original character
        GameCharacter warrior =
                new GameCharacter("Warrior", 100, 10, "Sword");

        // Clone the original character
        GameCharacter warrior1 = warrior.clone();
        GameCharacter warrior2 = warrior.clone();

        // Customize cloned characters
        warrior1.setHealth(80);
        warrior1.setLevel(12);

        warrior2.setHealth(60);
        warrior2.setLevel(15);

        System.out.println("Original Character:");
        warrior.displayCharacter();

        System.out.println("First Cloned Character:");
        warrior1.displayCharacter();

        System.out.println("Second Cloned Character:");
        warrior2.displayCharacter();
    }
}
```

### Output

```text
Original Character:
Name: Warrior
Health: 100
Level: 10
Weapon: Sword
------------------

First Cloned Character:
Name: Warrior
Health: 80
Level: 12
Weapon: Sword
------------------

Second Cloned Character:
Name: Warrior
Health: 60
Level: 15
Weapon: Sword
------------------
```

### Explanation

1. First, we create one original Warrior object.
2. We call `clone()` to create two copies of that object.
3. Both copies initially have the same property values as the original.
4. We update the health and level of each cloned character.
5. Changes to one character do not affect the other characters' primitive fields.

Notice that we did not create each Warrior using the constructor again. We copied the existing object.

---

## 4. Why do we use the Cloneable interface?

```java
class GameCharacter implements Cloneable
```

`Cloneable` is a marker interface.

It tells the `Object.clone()` method that this class allows cloning.

If a class does not implement `Cloneable` and we call `super.clone()`, Java throws `CloneNotSupportedException`.

Important points:
- `Cloneable` does not declare a `clone()` method.
- `Object.clone()` performs the actual copying.
- We override `clone()` to expose a suitable method for our class.

---

## 5. Shallow Copy vs Deep Copy

This is one of the most important interview topics related to Prototype.

### A. Shallow Copy

In a shallow copy:
- Primitive values are copied.
- Object references are copied, not the objects themselves.
- Both original and cloned objects refer to the same nested object.

Let's understand this with a game weapon example.

Suppose a character has a `Weapon` object.

```java
class Weapon {

    String name;

    Weapon(String name) {
        this.name = name;
    }
}
```

If we clone a character using `super.clone()`, the original and cloned characters share the same Weapon object.

```text
Original Character --------\
                            \
                             ---> Same Weapon Object
                            /
Cloned Character ----------/
```

So, if we change the weapon name through the cloned character, the original character will also see that change.

### B. Deep Copy

In a deep copy:
- A new object is created for nested objects as well.
- Original and cloned objects have separate nested objects.
- Changes to one object's nested data do not affect the other.

Example:

```java
class Weapon {

    String name;

    Weapon(String name) {
        this.name = name;
    }
}

class GameCharacter implements Cloneable {

    String name;
    int health;
    Weapon weapon;

    GameCharacter(String name, int health, Weapon weapon) {
        this.name = name;
        this.health = health;
        this.weapon = weapon;
    }

    @Override
    public GameCharacter clone() {

        try {
            GameCharacter copy =
                    (GameCharacter) super.clone();

            // Create a separate Weapon object
            copy.weapon = new Weapon(this.weapon.name);

            return copy;

        } catch (CloneNotSupportedException e) {
            throw new AssertionError(e);
        }
    }
}
```

Now the cloned character has its own Weapon object.

```text
Original Character ------> Weapon Object 1

Cloned Character --------> Weapon Object 2
```

Changes to one weapon will not affect the other.

---

## 6. Prototype Design Pattern Without Cloneable

We can also implement the Prototype pattern without using Java's `Cloneable` interface.

For example, we can define our own `copy()` method.

```java
class GameCharacter {

    private String name;
    private int health;
    private int level;

    public GameCharacter(String name, int health, int level) {
        this.name = name;
        this.health = health;
        this.level = level;
    }

    public GameCharacter copy() {
        return new GameCharacter(name, health, level);
    }
}
```

Here, `copy()` creates and returns a new object with the same values.

This approach gives us more control over how the object is copied.

For classes containing mutable nested objects, make sure the `copy()` method performs a deep copy when required.

---

## 7. Advantages of Prototype Design Pattern

1. Reduces repeated object creation and initialization code.
2. Useful when creating an object from scratch is expensive.
3. Makes it easy to create multiple objects with similar configurations.
4. Allows us to customize each copied
