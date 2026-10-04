# Week 14 — Lecture Content: Software Design — UML, Composition vs. Inheritance, SOLID

## 1. Reading a Basic UML Class Diagram
```
+---------------------+
|       Shape         |   <- class name (italic if abstract)
+---------------------+
| # area: double       |   <- attribute: # means protected, - private, + public
+---------------------+
| + computeArea(): double |  <- method
+---------------------+
          ^
          | (hollow triangle arrow = inheritance / "is-a")
+---------------------+
|       Circle         |
+---------------------+
| - radius: double     |
+---------------------+
| + computeArea(): double |
+---------------------+
```
A UML class box lists the class name, attributes (with visibility markers), and methods. An
open/hollow triangle arrow pointing from subclass to superclass denotes inheritance ("is-a"). A
diamond at one end of a line denotes composition/aggregation ("has-a") — a filled diamond for
composition (the part cannot outlive the whole) and an open diamond for the weaker aggregation.

## 2. Composition vs. Inheritance, Drawn
```
+--------+        +--------+              +---------+
|  Car   |◆------>| Engine |              |  Shape  |
+--------+  has-a +--------+              +---------+
                                                ^
                                                | is-a
                                          +-----------+
                                          |  Circle   |
                                          +-----------+
```
`Car` has-a `Engine` (composition, Week 3); `Circle` is-a `Shape` (inheritance, Week 5). The UML
notation makes the distinction visual, which is often clearer than code alone when discussing a
design with others before writing it.

## 3. "Is-A" vs. "Has-A" as a Design Decision
```java
// Tempting but wrong: Stack "is-a" ArrayList? It technically compiles...
public class Stack<T> extends ArrayList<T> { }   // now exposes get(index), remove(index), etc. —
                                                   // NOT what a stack's interface should allow

// Better: Stack "has-a" List internally, and exposes only push/pop.
public class Stack<T> {
    private List<T> items = new ArrayList<>();
    public void push(T item) { items.add(item); }
    public T pop() { return items.remove(items.size() - 1); }
}
```
Inheritance should model a genuine "is-a" relationship where every superclass method and
invariant should hold for the subclass too. `Stack extends ArrayList` compiles, but a `Stack`
should not expose arbitrary indexed access — the relationship is really "has-a," and composition
is the correct tool, even though inheritance was syntactically available. When in doubt, and
especially when the relationship is not unmistakably "is-a," favor composition.

## 4. Single Responsibility Principle (SRP)
```java
// Before: one class doing far too much.
public class Report {
    public void computeTotals() { /* ... */ }
    public void formatAsHtml() { /* ... */ }
    public void saveToDatabase() { /* ... */ }
}

// After: each responsibility in its own class.
public class ReportCalculator { public void computeTotals() { /* ... */ } }
public class ReportFormatter { public String formatAsHtml(ReportCalculator r) { /* ... */ return ""; } }
public class ReportRepository { public void save(ReportCalculator r) { /* ... */ } }
```
A class with one clear job is easier to understand, test, and change without breaking unrelated
behavior. `Report` above changes for three unrelated reasons (a calculation tweak, an HTML
format tweak, a storage tweak) — splitting it means each kind of change touches only one class.

## 5. Open/Closed Principle (OCP)
```java
// Before: a switch/instanceof chain that must be edited for every new shape.
public double totalArea(List<Object> shapes) {
    double total = 0;
    for (Object s : shapes) {
        if (s instanceof Circle c) total += Math.PI * c.getRadius() * c.getRadius();
        else if (s instanceof Rectangle r) total += r.getWidth() * r.getHeight();
        // every new shape type means editing this method again
    }
    return total;
}

// After: polymorphism (Weeks 7-8) — open for extension, closed for modification.
public double totalArea(List<Shape> shapes) {
    double total = 0;
    for (Shape s : shapes) {
        total += s.area();   // a brand-new Shape subclass needs NO change here
    }
    return total;
}
```
"Open for extension, closed for modification": a well-designed abstraction lets you add new
behavior (a new `Shape` subclass) without editing existing, already-tested code. The
`instanceof`-chain version violates this; the polymorphic version, built from exactly what Weeks
7–9 covered, satisfies it.

## 6. In-Class Exercise
Take a provided class that mixes input validation, business logic, and console output in one
method, and refactor it into three smaller classes following SRP. Separately, take a provided
`instanceof` chain over a small shape hierarchy and refactor it to use polymorphism instead,
satisfying OCP.
