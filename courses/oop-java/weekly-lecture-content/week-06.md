# Week 6 — Lecture Content: Inheritance II — Overriding vs. Overloading, `@Override`, `Object`

## 1. Overriding vs. Overloading, Side by Side
```java
public class Animal {
    public String makeSound() {                 // no parameters
        return "Some generic sound";
    }
}

public class Dog extends Animal {
    @Override
    public String makeSound() {                 // SAME signature — this OVERRIDES Animal's version
        return "Woof!";
    }

    public String makeSound(int times) {         // DIFFERENT signature — this OVERLOADS, a new method
        return "Woof! ".repeat(times);
    }
}
```
**Overriding** redefines an inherited method with the *exact same* signature (name + parameter
types) — calling it through any reference actually pointing to a `Dog` now runs `Dog`'s version.
**Overloading** declares a *different* method that merely shares a name — it does not replace
anything, it adds a sibling method distinguished by its parameter list. The two are easy to
confuse and have very different effects on a program's behavior.

## 2. `@Override`: Letting the Compiler Check Your Intent
```java
public class Animal {
    public String makeSound() { return "..."; }
}

public class Dog extends Animal {
    @Override
    public String makeSond() {    // typo in the method name!
        return "Woof!";
    }
}
```
With `@Override` present, the line above is a **compile error**: `@Override` tells the compiler
"this method must override something in a superclass," and `makeSond` (typo) overrides nothing —
it would otherwise have silently compiled as an unrelated new method, and `Dog` objects would
keep barking with `Animal`'s unoverridden `makeSound()` with no warning at all. This is exactly
the mechanism that would have caught Week 2's `equals(Rectangle other)` bug immediately:
```java
public class Rectangle {
    @Override
    public boolean equals(Rectangle other) {   // COMPILE ERROR with @Override present:
        // ...                                 // Object has no equals(Rectangle) to override
    }
}
```
Always annotate an intended override with `@Override` — it costs nothing and converts an entire
class of silent bugs into compile errors.

## 3. `Object`: The Universal Superclass
```java
public class Dog {   // no "extends" written — but this is shorthand for:
}
public class Dog extends Object {   // ...exactly this, implicitly
}
```
Every class that does not explicitly `extends` another class implicitly extends `java.lang.Object`
— there is no way to opt out. That means every class, from the very first one you ever wrote,
already has (and may override) `toString()`, `equals(Object)`, `hashCode()`, and a handful of
other methods (`getClass()`, `wait()`/`notify()`, used in later courses), whether or not it ever
declares `extends` at all.

## 4. What You Already Have, Before Writing Any Code
```java
public class Point {
    private int x, y;
    public Point(int x, int y) { this.x = x; this.y = y; }
}

Point p = new Point(1, 2);
System.out.println(p);            // Point@<hashcode> — Object's default toString()
System.out.println(p.equals(p));  // true — Object's default equals() is reference ("==") identity
```
Before any override, `toString()` prints the unhelpful class-name-plus-hash-code form, and
`equals()` only considers two references equal if they point to the *exact same object* — the
same default behavior responsible for the Week 2 lesson that a meaningful `equals()` almost always
needs to be written deliberately.

## 5. In-Class Exercise
Given five method pairs (provided on a handout — some same-signature, some differing only in
return type, some differing in parameter list, one with a parameter-type typo), classify each
pair as "override," "overload," or "does not compile," and add `@Override` to every pair that
should be an override. For one deliberately-broken pair, explain what the compiler message says
and why.
