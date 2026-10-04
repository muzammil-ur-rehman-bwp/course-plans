# Week 7 — Lecture Content: Polymorphism I — Dynamic Dispatch, Upcasting/Downcasting, `instanceof`

## 1. Every Instance Method Is Virtual by Default
```java
public class Animal {
    public String makeSound() { return "..."; }
}

public class Dog extends Animal {
    @Override
    public String makeSound() { return "Woof!"; }
}

Animal a = new Dog();          // upcast: a Dog stored through an Animal-typed reference
System.out.println(a.makeSound());   // "Woof!" — Dog's version runs, NOT Animal's
```
Unlike C++, where a method is only dynamically dispatched if explicitly declared `virtual`, every
Java instance method is virtual **by default**. The only ways to turn dynamic dispatch *off* for
a method are to mark it `final` (cannot be overridden at all), `private` (not inherited, so there
is nothing to override), or `static` (resolved by the reference's declared type, not the object's
actual type — static methods are not polymorphic). No separate keyword is needed to get
polymorphic behavior; you have to opt *out* of it, not into it.

## 2. Upcasting: Always Safe
```java
Dog rex = new Dog();
Animal a = rex;        // upcast — implicit, no cast syntax needed, always safe
```
Treating a more specific object through a less specific (superclass) reference is called
**upcasting**. It always succeeds, because a `Dog` genuinely *is* an `Animal` — nothing about the
object changes, only the type of reference used to look at it.

## 3. No Object Slicing in Java
```java
// C++ (for contrast — NOT Java): passing a Derived object by value as a Base parameter
// COPIES only the Base part, "slicing off" Derived's extra state and overrides.
// void handle(Base b) { ... }   // b is a truncated Base copy, not the real Derived object

// Java: there is no by-value object copy at all.
void handle(Animal a) {
    System.out.println(a.makeSound());   // still dispatches to Dog's override — nothing is sliced
}
handle(rex);   // rex (a Dog) is passed as a reference; the SAME Dog object is used inside handle
```
A Java variable of a class type never holds the object itself — it holds a **reference** to it.
Passing `rex` to `handle(Animal a)` copies the reference, not the object, so `a` inside `handle`
still refers to the one real `Dog`, with all of `Dog`'s overrides intact. The slicing problem C++
students must watch for when passing polymorphic objects by value simply does not exist in Java.

## 4. Downcasting: Requires an Explicit, Checked Cast
```java
Animal a = new Dog();
Dog d = (Dog) a;        // downcast — explicit cast required
d.fetch();              // now safe to call Dog-only methods

Animal other = new Animal();
Dog bad = (Dog) other;  // compiles, but throws ClassCastException at RUNTIME — other isn't a Dog
```
Going back down from a superclass reference to a subclass reference is a **downcast**, and the
compiler cannot verify it is safe on its own — it only compiles with an explicit cast, and the
*runtime* throws `ClassCastException` if the object's actual type doesn't match.

## 5. `instanceof`: Checking Before You Downcast
```java
Animal a = new Dog();

if (a instanceof Dog) {        // check the ACTUAL runtime type before downcasting
    Dog d = (Dog) a;
    d.fetch();
}

// Java 16+ pattern-matching form — combines the check and the cast:
if (a instanceof Dog d) {
    d.fetch();                 // d is already a Dog here, no separate cast line needed
}
```
`instanceof` checks an object's actual runtime type, letting you guard a downcast instead of
risking a `ClassCastException`. The modern pattern-matching form folds the check and the cast
into one expression, binding the already-cast variable (`d`) directly in the `if`'s scope.

## 6. In-Class Exercise
Given an `Animal[]` array holding a mix of `Dog` and `Cat` objects (both subclasses of `Animal`
from Week 5), write a loop that calls `makeSound()` polymorphically on every element, then uses
`instanceof` pattern matching to safely call `fetch()` only on the elements that are actually
`Dog`s.
