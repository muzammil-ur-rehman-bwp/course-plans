# Week 15 — Lecture Content: Debugging & Testing OOP Code

## 1. Reading a Stack Trace Across a Class Hierarchy
```java
public abstract class Shape {
    public abstract double area();
    public String report() { return "Area: " + area(); }
}

public class BrokenCircle extends Shape {
    private double radius;
    public BrokenCircle(double radius) { this.radius = radius; }

    @Override
    public double area() {
        Shape helper = null;
        return helper.area();   // NullPointerException, right here
    }
}

Shape s = new BrokenCircle(5);
System.out.println(s.report());
```
```
Exception in thread "main" java.lang.NullPointerException
    at BrokenCircle.area(BrokenCircle.java:10)
    at Shape.report(Shape.java:3)
    at Main.main(Main.java:2)
```
Read a stack trace **top to bottom**: the top frame is where the exception was actually thrown
(`BrokenCircle.area`, line 10) — not where it was caught or printed. `Shape.report` (frame two)
only *called* the method that failed; it is not itself the bug. Across a hierarchy, the top frame
tells you exactly which override ran and where inside it things went wrong, which is often enough
to find the fix without any other tooling.

## 2. OOP Bug Checklist
```java
// 1. Missing @Override — silently creates an overload instead of an override (Weeks 2, 6).
public boolean equals(MyClass other) { ... }     // should be equals(Object obj), with @Override

// 2. equals() without a matching hashCode() (Week 2) — breaks HashMap/HashSet lookups.
@Override public boolean equals(Object o) { ... }
// hashCode() left as Object's default — WRONG if equals() was overridden

// 3. Confusing an interface default method with abstract-class state (Weeks 8-9).
public interface HasCounter {
    int count = 0;         // this is implicitly `public static final` — NOT per-instance state!
}

// 4. A raw generic type producing an unchecked warning (Week 10).
List rawList = new ArrayList();   // should be List<SomeType>
```
Keep this list handy while debugging: these four mistakes all *compile*, and all four produce
behavior that only looks wrong once the program runs — exactly the kind of bug a compiler cannot
catch for you, but that a quick self-review usually can.

## 3. Testing Concepts, Conceptually (No JUnit Setup Required)
```java
// What a JUnit-style test method looks like, conceptually — this is NOT a real JUnit import,
// just the shape of the idea: a method that calls code and asserts an expected result.
public class ShapeTests {
    public static void testCircleArea() {
        Shape c = new Circle(2);
        double expected = Math.PI * 4;
        assert Math.abs(c.area() - expected) < 0.0001 : "Circle area is wrong";
    }
}
```
A unit test calls a small piece of code and checks ("asserts") that the result matches what's
expected. A real JUnit project wraps this in `@Test` annotations and a test runner; conceptually,
every JUnit test is still just "call the method, assert the expected result," which is the part
that matters for designing testable classes — a topic a dedicated software-testing course will
cover with real tooling.

## 4. A Hand-Rolled Test Harness
```java
public class ManualTests {
    private static int passed = 0, failed = 0;

    private static void check(String label, boolean condition) {
        if (condition) {
            passed++;
        } else {
            failed++;
            System.out.println("FAILED: " + label);
        }
    }

    public static void main(String[] args) {
        Shape circle = new Circle(2);
        check("circle area", Math.abs(circle.area() - Math.PI * 4) < 0.0001);

        Shape rect = new Rectangle(3, 4);
        check("rectangle area", rect.area() == 12.0);

        Object a = new Rectangle(3, 4);
        Object b = new Rectangle(3, 4);
        check("rectangle equals", a.equals(b));          // exercises Week 2's equals() override
        check("rectangle hashCode matches", a.hashCode() == b.hashCode());

        System.out.println(passed + " passed, " + failed + " failed");
    }
}
```
This kind of small, hand-rolled harness — no framework required — exercises a class's
constructors, its overridden `equals`/`hashCode`, and its polymorphic behavior (`area()` through
the abstract `Shape` type) in one place, and is good practice before adopting a real testing
framework in a later course.

## 5. In-Class Exercise
Given a three-level class hierarchy (`Shape` → `Polygon` → `Rectangle`) with a deliberately
planted bug in the middle class's override, trace the provided stack trace to the exact line and
method responsible, then write two `check(...)`-style assertions in a `ManualTests` class that
would have caught the bug before it shipped.
