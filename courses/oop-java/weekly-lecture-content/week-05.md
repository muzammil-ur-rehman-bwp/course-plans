# Week 5 — Lecture Content: Inheritance I — `extends`, Access Control, Constructor Chaining

## 1. `extends`: What a Subclass Inherits
```java
public class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public String makeSound() {
        return name + " makes a sound";
    }
}

public class Dog extends Animal {   // Dog IS-A Animal
    public Dog(String name) {
        super(name);                // calls Animal's constructor — see section 3
    }
    // Dog automatically has makeSound() and the inherited `name` field, with no code written here
}
```
```java
Dog d = new Dog("Rex");
System.out.println(d.makeSound());   // Rex makes a sound — inherited, unchanged, from Animal
```
`class Dog extends Animal` makes `Dog` a **subclass** of `Animal` (the **superclass**). A subclass
inherits every non-`private` field and method its superclass declares, and can be used wherever
that behavior is needed without rewriting it.

## 2. Access Control Across Inheritance
```java
public class Animal {
    private String secretId;     // NOT visible to Dog at all, even though Dog extends Animal
    protected String name;       // visible to Animal AND any subclass, but not to outside code
    public String species;       // visible everywhere
}

public class Dog extends Animal {
    public void describe() {
        System.out.println(name);       // OK — protected, visible to subclasses
        // System.out.println(secretId); // COMPILE ERROR — private, not visible even here
    }
}
```
`protected` is the access level inheritance adds meaning to: hidden from unrelated outside code,
but visible inside the superclass **and** every subclass. `private` stays private even from a
subclass — a subclass only ever sees a superclass's `private` state indirectly, through the
superclass's own public/protected methods.

## 3. Constructor Chaining with `super(...)`
```java
public class Vehicle {
    protected String licensePlate;

    public Vehicle(String licensePlate) {
        this.licensePlate = licensePlate;
    }
}

public class Car extends Vehicle {
    private int numDoors;

    public Car(String licensePlate, int numDoors) {
        super(licensePlate);      // MUST be the first statement — invokes Vehicle's constructor
        this.numDoors = numDoors;
    }
}
```
A subclass constructor must initialize its inherited state by invoking a superclass constructor —
explicitly, via `super(args)` as the very first statement, or implicitly (if `super(...)` is
omitted, Java inserts a call to the superclass's **no-argument** constructor). If `Vehicle` had no
no-argument constructor and `Car` omitted `super(licensePlate)`, the class would fail to compile:
there would be no way to initialize `Vehicle`'s part of the new `Car`.

## 4. Single Inheritance Only
```java
// Legal — one superclass:
public class Car extends Vehicle { }

// NOT legal in Java — a class may extend at most one superclass:
// public class Car extends Vehicle, Insurable { }   // COMPILE ERROR
```
Java deliberately allows a class to `extends` only **one** superclass. C++ permits a class to
inherit from several base classes directly, which can create the *diamond problem* — ambiguity
about which ancestor's state a most-derived object actually has, when two base classes share a
common ancestor. Java avoids that ambiguity by construction: single class inheritance, plus
**multiple interface implementation** (Week 9) for any behavior a class needs to share with
unrelated types.

## 5. A Small Two-Level Hierarchy
```java
public class Animal {
    protected String name;
    public Animal(String name) { this.name = name; }
    public String makeSound() { return name + " makes a generic sound"; }
}

public class Dog extends Animal {
    public Dog(String name) { super(name); }
    public String fetch() { return name + " fetches the ball"; }
}

public class Cat extends Animal {
    public Cat(String name) { super(name); }
    public String scratch() { return name + " scratches the post"; }
}
```
`Dog` and `Cat` each inherit `name` and `makeSound()` from `Animal`, and each adds behavior of its
own. Neither needs to redeclare `name` or reimplement `makeSound()` to get it.

## 6. In-Class Exercise
Design a `Shape` class with a `protected double area` field and a constructor that sets it, and a
`Square` subclass whose constructor computes the area from a side length and passes it to
`super(...)`. Explain, in a comment, what access level `area` would need for a *different*
package's subclass to see it (a preview of the full access-control picture, covered fully in a
later course).
