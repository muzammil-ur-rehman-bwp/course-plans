# Week 2 — Lecture Content: Constructors in Depth; `equals`/`hashCode`/`toString`; No Destructors

## 1. Overloaded Constructors
```java
public class Rectangle {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = Math.max(width, 1);
        this.height = Math.max(height, 1);
    }

    public Rectangle(double side) {        // overload: a square
        this(side, side);                  // constructor chaining — must be the FIRST statement
    }

    public Rectangle() {                   // overload: default 1x1 square
        this(1, 1);
    }
}
```
Java lets a class declare several constructors with different parameter lists — **overloaded
constructors**. `this(...)` calls another constructor of the *same* class, and if used, it must be
the very first statement in the constructor body. Chaining this way means the validation logic
in the two-argument constructor runs exactly once, no matter which constructor the caller used.

## 2. The Default Constructor
```java
public class Empty { }   // the compiler supplies a public, no-argument constructor for free

public class Rectangle {
    public Rectangle(double width, double height) { /* ... */ }
    // Java no longer supplies a no-argument constructor here —
    // once you write ANY constructor, the free default one disappears.
}
```
If a class declares no constructor at all, Java supplies a no-argument default constructor. As
soon as you write even one constructor yourself, that free one is gone — if you still want a
no-argument constructor, you must write it explicitly (as `Rectangle()` did in section 1).

## 3. Overriding `toString()`
```java
public class Rectangle {
    private double width, height;
    // ... constructors ...

    @Override
    public String toString() {
        return "Rectangle[width=" + width + ", height=" + height + "]";
    }
}

Rectangle r = new Rectangle(3, 4);
System.out.println(r);   // prints: Rectangle[width=3.0, height=4.0] — calls toString() automatically
```
Every class inherits a default `toString()` from `Object` that prints something unhelpful like
`Rectangle@1b6d3586`. Overriding it gives `System.out.println` and string concatenation a
meaningful representation of the object.

## 4. Overriding `equals(Object)` — and the Classic Bug
```java
public class Rectangle {
    private double width, height;

    // WRONG — this OVERLOADS Object's equals, it does not OVERRIDE it.
    // The parameter type must be exactly Object, not Rectangle.
    public boolean equals(Rectangle other) {
        return width == other.width && height == other.height;
    }
}
```
```java
public class Rectangle {
    private double width, height;

    @Override                                 // @Override catches the mistake above at compile time
    public boolean equals(Object obj) {       // parameter type MUST be Object
        if (this == obj) return true;
        if (!(obj instanceof Rectangle)) return false;
        Rectangle other = (Rectangle) obj;
        return width == other.width && height == other.height;
    }
}
```
Writing `equals(Rectangle other)` *compiles* — but it silently creates a second, overloaded
method instead of overriding `Object`'s `equals(Object)`. Code that calls `equals` through an
`Object` reference (which is exactly what collections do internally) still uses `Object`'s
default identity comparison, and the bug goes unnoticed until equality checks mysteriously fail.
Always write `@Override` on an intended override — the compiler then rejects a mismatched
signature instead of silently accepting it as a new overload.

## 5. The `equals`/`hashCode` Contract
```java
public class Rectangle {
    private double width, height;

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Rectangle)) return false;
        Rectangle other = (Rectangle) obj;
        return width == other.width && height == other.height;
    }

    @Override
    public int hashCode() {
        return java.util.Objects.hash(width, height);   // must agree with equals' fields
    }
}
```
Java's contract: **if two objects are `equals()`, they must return the same `hashCode()`.**
`HashMap`, `HashSet`, and similar classes rely on this — they use `hashCode()` to find the right
"bucket" and `equals()` to confirm a match within it. Override one without the other and a class
can silently misbehave inside a `HashSet`/`HashMap` (e.g. two "equal" objects both get stored, each
findable only sometimes). Build `hashCode()` from exactly the same fields `equals()` compares.

## 6. No Destructors — Garbage Collection Recap
```java
public class TempFile {
    public TempFile() {
        System.out.println("created");
    }
    // no destructor — Java has no such thing.
}

TempFile t = new TempFile();
t = null;   // the object is now unreachable; the JVM's garbage collector will reclaim it
            // eventually, at a time you do not control and should not depend on
```
Unlike C++, Java has **no destructors** and no Rule of Three to track: the JVM's garbage collector
automatically reclaims memory for any object no reference points to anymore, on its own schedule.
This is a direct continuation of the prerequisite course's reference/`null` discussion — you never
manually free a Java object, and you never need a class whose sole job is to clean one up.

## 7. In-Class Exercise
Write a `Point` class with fields `x`, `y`; two constructors (`Point(double x, double y)` and a
no-argument `Point()` that chains to it with `this(0, 0)`); and correct, `@Override`-annotated
`equals`, `hashCode`, and `toString` methods. Verify in `main` that `new Point(1, 2).equals(new
Point(1, 2))` is `true` and that both objects report the same `hashCode()`.
