# Week 8 — Lecture Content: Polymorphism II — Abstract Classes; Midterm Review

## 1. Why Some Superclasses Shouldn't Be Instantiated
```java
public class Shape {
    public double area() {
        return 0;   // meaningless default — every real shape needs its own formula
    }
}
```
A plain `Shape` with a placeholder `area()` invites a useless object (`new Shape()`) and a wrong
answer (`0`) that's easy to forget to override. What we actually want is a `Shape` that forces
every concrete subclass to supply its own `area()`, and that can never be instantiated on its own.

## 2. The `abstract` Keyword
```java
public abstract class Shape {
    public abstract double area();     // abstract method: no body, must be implemented below

    public String describe() {         // ordinary concrete method: shared by every subclass
        return "This shape has area " + area();
    }
}

// Shape s = new Shape();   // COMPILE ERROR — cannot instantiate an abstract class
```
An `abstract` class may mix **abstract methods** (declared with no body, ending in `;`) and
ordinary **concrete methods** with full implementations shared by every subclass. Marking the
class itself `abstract` makes the compiler refuse `new Shape()` — it exists only to be extended,
never instantiated directly.

## 3. Concrete Subclasses Must Implement Every Abstract Method
```java
public class Circle extends Shape {
    private double radius;
    public Circle(double radius) { this.radius = radius; }

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

public class Rectangle extends Shape {
    private double width, height;
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double area() {
        return width * height;
    }
}
```
Any concrete (non-abstract) subclass of `Shape` *must* override `area()` — the compiler enforces
it, so there is no way to end up with a `Circle` that silently inherits a meaningless default area.

## 4. Driving Through the Abstract Type
```java
import java.util.List;

List<Shape> shapes = List.of(new Circle(2), new Rectangle(3, 4));

for (Shape s : shapes) {
    System.out.println(s.describe());   // dispatches to Circle's or Rectangle's area() correctly
}
```
This is the payoff of Weeks 7–8 together: code written entirely in terms of `Shape` — a type that
can never itself be instantiated — correctly calls each concrete subclass's own `area()` through
dynamic dispatch, with no `instanceof` chain needed anywhere in this loop.

## 5. Abstract Class vs. a Plain Superclass With Default Behavior
An abstract class is the right tool exactly when: (a) there is no sensible default implementation
for at least one method (`Shape` has no sensible generic `area()`), and (b) instantiating the base
type directly would be meaningless (there is no such thing as a plain, non-specific "Shape"
object with a real area). Week 9 introduces `interface`, a second tool for a related but distinct
problem — a contract with *no* shared state at all.

## 6. Midterm Review: What to Revisit
- Encapsulation (Week 1): private fields, getters/setters, `this`, immutability.
- Constructors & `Object` methods (Week 2): `this(...)` chaining, `equals`/`hashCode`/`toString`,
  no destructors.
- Composition (Week 3): "has-a," delegation.
- Static vs. instance (Week 4): shared vs. per-object state, utility classes.
- Inheritance I & II (Weeks 5–6): `extends`, access control, `super(...)`, overriding vs.
  overloading, `@Override`.
- Polymorphism I & II (Weeks 7–8): dynamic dispatch, upcasting/downcasting, `instanceof`, abstract
  classes.

## 7. In-Class Exercise
Design an abstract `Employee` class with an abstract `double computePay()` method and a concrete
`String describe()` method that calls it. Implement two concrete subclasses, `SalariedEmployee`
and `HourlyEmployee`, each with its own `computePay()`, and drive both through a `List<Employee>`.
