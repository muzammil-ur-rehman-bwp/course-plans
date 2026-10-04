# Week 1 — Lecture Content: Classes Recap & Encapsulation

> This course picks up exactly where Programming Fundamentals (Java) left off. That course's
> Week 13 gave you private fields, a constructor, and static-member basics — and explicitly named
> inheritance, polymorphism, interfaces, and abstract classes as belonging to "the follow-on
> Object-Oriented Programming course." This is that course. This week we tighten up encapsulation;
> from Week 2 onward we build everything that course deliberately left out.

## 1. Why Not Just Make Everything `public`?
```java
public class BadAccount {
    public double balance;   // public field — ANY code anywhere can set this to anything
}

BadAccount acc = new BadAccount();
acc.balance = -500.0;   // nothing stops an invalid, nonsensical balance
```
When a field is `public`, nothing enforces the rules that should govern it. **Encapsulation**
means making fields `private` and exposing controlled access only through the class's own public
methods, so the class itself can enforce its own rules — the only code that may directly touch
`balance` is code written inside `BadAccount`.

## 2. Getters and Setters
```java
public class Temperature {
    private double celsius;

    public Temperature(double celsius) {
        setCelsius(celsius);
    }

    public double getCelsius() {           // getter: reads the private field
        return celsius;
    }

    public void setCelsius(double value) {  // setter: validates before writing
        if (value < -273.15) {
            this.celsius = -273.15;         // clamp to absolute zero instead of storing nonsense
        } else {
            this.celsius = value;
        }
    }
}
```
A **getter** reads a private field; a **setter** writes one and is the natural place to validate a
new value before it is stored. Not every private field needs both — some are read-only from
outside the class (only a getter), and some should never change after construction at all (no
setter at all, set once in the constructor).

## 3. The `this` Keyword
```java
public class Point {
    private double x;
    private double y;

    public Point(double x, double y) {
        this.x = x;   // this.x is the field; x (no prefix) is the constructor parameter
        this.y = y;
    }
}
```
Inside a constructor or method, a parameter can legally share a field's name. `this.x` always
refers to the current object's field; plain `x` refers to the parameter shadowing it. Without
`this.x = x;`, the line `x = x;` would just assign the parameter to itself and the field would
stay unset — a classic beginner bug `this` exists to prevent.

## 4. Immutability Basics
```java
public final class Point {
    private final double x;   // final: can only be assigned once, here in the constructor
    private final double y;

    public Point(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }
    // no setters at all — once created, a Point can never change
}
```
A field marked `final` can be assigned exactly once. A class with only `final` fields, set once
in the constructor, and no setters, is **immutable**: once created, its state can never change.
Immutable objects are easier to reason about (no code anywhere can corrupt them after the fact)
and are safe to share freely — a theme that recurs when we reach collections and multithreading
topics in later courses.

## 5. Refactoring a Poorly-Encapsulated Class
```java
// Before: no encapsulation at all.
public class RawAccount {
    public double balance;   // any code anywhere can set this to anything
}

// After: encapsulated and validated.
public class Account {
    private double balance;

    public Account(double startingBalance) {
        this.balance = Math.max(startingBalance, 0.0);
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;   // rejected: invalid amount or insufficient funds
    }
}
```
The *shape* of `Account` (a private field, a validating constructor, a getter, and validated
mutator methods instead of a blanket setter) is the pattern you will reuse and extend for every
class this semester, starting with constructors in depth next week.

## 6. In-Class Exercise
Take the following class and refactor it so that `balance` is private, is only modifiable through
a validated `deposit`/`withdraw` pair of methods, and add a second, immutable class
`AccountId` (a `final` `String id` field, no setters) that an `Account` could later use to
identify itself:
```java
public class Account {
    public double balance;
}
```
