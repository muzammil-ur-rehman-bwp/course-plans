# Week 9 — Lecture Content: Interfaces

> Midterm Exam covers Weeks 1–8 this week. The material below is taught in the remaining lecture
> time after the exam.

## 1. The `interface` Keyword
```java
public interface Payable {
    double computeAmountDue();   // no body — an interface method is implicitly abstract
}
```
An `interface` declares a contract: a set of method signatures with no implementation (before
default methods, section 4) and no instance fields. Any class may declare that it fulfills this
contract with `implements`.

## 2. `implements`
```java
public class Employee implements Payable {
    private double salary;
    public Employee(double salary) { this.salary = salary; }

    @Override
    public double computeAmountDue() {
        return salary;
    }
}

public class Invoice implements Payable {
    private double amount;
    public Invoice(double amount) { this.amount = amount; }

    @Override
    public double computeAmountDue() {
        return amount;
    }
}
```
`Employee` and `Invoice` are entirely unrelated classes (neither extends the other, and neither
has a common custom superclass) — yet both satisfy `Payable`, so both can be used anywhere a
`Payable` is expected:
```java
import java.util.List;

List<Payable> bills = List.of(new Employee(3000), new Invoice(450));
double total = 0;
for (Payable p : bills) {
    total += p.computeAmountDue();   // dispatches correctly regardless of concrete class
}
```

## 3. Multiple Interface Implementation — No Diamond Problem
```java
public interface Printable {
    void print();
}
public interface Serializable2 {   // (named to avoid clashing with java.io.Serializable)
    String serialize();
}

public class Report implements Payable, Printable, Serializable2 {
    @Override public double computeAmountDue() { return 0; }
    @Override public void print() { System.out.println("Printing report"); }
    @Override public String serialize() { return "report-data"; }
}
```
A class may `implements` as many interfaces as it needs, while still `extends`-ing at most one
superclass (Week 5). This is Java's deliberate alternative to C++'s multiple class inheritance:
because a (pre-default-method) interface carries no state and no implementation of its own, there
is no ambiguity about which ancestor's data a `Report` object actually has — the diamond problem
from Week 5 cannot arise this way. Even with default methods (next section), if two interfaces
supply conflicting defaults, Java forces the implementing class to resolve the conflict explicitly
rather than silently picking one.

## 4. `default` Methods (Brief)
```java
public interface Greetable {
    String getName();

    default String greet() {                 // default method: has a body
        return "Hello, " + getName() + "!";
    }
}

public class Person implements Greetable {
    private String name;
    public Person(String name) { this.name = name; }

    @Override
    public String getName() { return name; }
    // greet() is inherited from the interface with no extra code needed
}
```
A `default` method lets an interface supply a method *with* a body, so existing implementers keep
compiling unchanged when a new method is added to the interface later. Used sparingly — an
interface is still primarily a contract, not a place to hide significant shared state or logic
(that remains the job of an abstract class, or composition).

## 5. Interfaces vs. Abstract Classes
| | Abstract class (Week 8) | Interface |
|---|---|---|
| Shared state (instance fields) | Yes | No |
| Shared implementation | Yes, freely | Only via `default` methods |
| How many a class may use | One (`extends`) | Many (`implements`) |
| Models | "is-a," with shared internals | "can-do," a capability/contract |

Choose an abstract class when subclasses share real state or substantial implementation and
genuinely form an "is-a" hierarchy. Choose an interface when unrelated classes need to be used
interchangeably through a shared contract, or when a class already extends something else but
still needs to advertise a capability.

## 6. In-Class Exercise
Design an interface `Drivable` with `void accelerate()` and `void brake()`. Implement it in two
unrelated classes, `Car` and `Bicycle` (neither extending the other), and write a method
`testDrive(Drivable d)` that calls both methods polymorphically through the interface type.
