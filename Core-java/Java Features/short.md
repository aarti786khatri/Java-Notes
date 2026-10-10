# Java OOP Concepts: Quick Revision

## 1. Encapsulation
- Wrapping data and methods into one class.
- Make variables `private` and provide controlled access through methods.
- **Example:** Bank account balance.
- **Remember:** Protect data.

## 2. Abstraction
- Hiding implementation details and showing only essential functionality.
- Achieved using abstract classes and interfaces.
- **Example:** We use an ATM without knowing its internal processing.
- **Remember:** Hide implementation details.

## 3. Inheritance
- One class acquires accessible properties and behaviors from another class.
- Uses the `extends` keyword for class inheritance.
- **Example:** `Dog extends Animal`.
- **Remember:** Code reuse and an is-a relationship.

## 4. Polymorphism
- Means many forms.
- One method name or common reference can represent different behaviors.
- **Compile-time:** Method overloading.
- **Runtime:** Method overriding.
- **Example:** Different payment methods implement `pay()` differently.
- **Remember:** Different behaviors.

## 5. Association
- A general relationship between two independent objects.
- **Example:** Teacher and student.
- **Remember:** Objects interact.

## 6. Aggregation
- A weak has-a relationship.
- The contained object can exist independently.
- **Example:** Department and employees.
- **Remember:** Independent parts.

## 7. Composition
- A strong has-a relationship.
- The containing object manages its component objects as part of its lifecycle.
- **Example:** Order and order items.
- **Remember:** Strong ownership.

## 8. Types of Inheritance
- **Single:** One parent, one child.
- **Multilevel:** A chain of inheritance.
- **Hierarchical:** Multiple children share one parent.
- **Multiple:** One child has multiple parents. Java does not support this through classes, but supports multiple interfaces.
- **Hybrid:** A combination of inheritance types. Supported combinations can use interfaces.

## 9. Important Keywords
- `this`: Refers to the current object.
- `super`: Accesses eligible parent members or invokes a parent constructor.
- `extends`: Inherits a class.
- `implements`: Implements an interface.
- `private`: Restricts direct access to the declaring class.
- `protected`: Allows access within the same package and to subclasses, subject to access rules.
- `static`: Belongs to the class rather than an individual instance.

## 10. Inheritance vs Composition
- **Inheritance:** Is-a relationship. Example: `Dog` is an `Animal`.
- **Composition:** Has-a relationship. Example: `Car` has an `Engine`.

## Quick Memory Trick

- Encapsulation = Protect data
- Abstraction = Hide implementation
- Inheritance = Reuse behavior
- Polymorphism = Many forms
- Association = Interaction
- Aggregation = Independent parts
- Composition = Strong ownership
