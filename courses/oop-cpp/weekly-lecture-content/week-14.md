# Week 14 — Lecture Content: Software Design — UML, Composition vs. Inheritance, SOLID

## 1. Reading a Basic UML Class Diagram
```
+---------------------+
|       Shape          |   <- class name
+---------------------+
| - name_: string      |   <- attributes (- = private, + = public, # = protected)
+---------------------+
| + area(): double      |   <- methods
| + draw(): void         |
+---------------------+
          ^
          |  (hollow triangle arrow = inheritance, "is-a")
+---------------------+
|       Circle          |
+---------------------+
| - radius_: double     |
+---------------------+
| + area(): double       |
+---------------------+

+---------------------+        +---------------------+
|        Car            |------>|       Engine          |   (filled diamond = composition, "has-a")
+---------------------+        +---------------------+
```
A UML class diagram has three parts per class: its name, its attributes (with visibility
markers), and its methods. A **hollow-triangle arrow** pointing from a derived class to its base
class denotes inheritance (this week's `Circle → Shape` matches Weeks 8–9's hierarchy exactly); a
**filled-diamond line** from a class to another denotes composition (`Car → Engine` matches Week
5). Being able to read (and sketch) this notation lets you communicate a design before writing
any code, and lets you read other people's designs — including library/framework documentation
that uses the same notation.

## 2. Composition vs. Inheritance: A Design Decision, Not Just Syntax
```cpp
// Is-a? Inheritance is appropriate:
class Square : public Rectangle { /* a Square genuinely IS a kind of Rectangle... mostly */ };

// Has-a? Composition is appropriate:
class Car { Engine engine_; /* a Car HAS an Engine; a Car is NOT a kind of Engine */ };

// Tempting but WRONG — inheriting just to reuse code, with no real is-a relationship:
class Stack : public std::vector<int> {
    // Exposes ALL of vector's interface (insert-in-the-middle, etc.) that breaks "stack" semantics
};
```
The `Stack : public std::vector<int>` example is a classic design smell: it reuses `vector`'s
implementation, but a `Stack` is not honestly "a kind of" `vector` — it should only expose
push/pop/top, not arbitrary insertion. The fix (as built correctly in Week 11) is composition: a
`Stack` **has a** `std::vector` as a private implementation detail, exposing only the narrower
interface a stack should have. **Default to composition**; reach for inheritance only when the
derived type must be substitutable for the base type everywhere the base type is used (a stricter
test than merely "wants to reuse some code").

## 3. The Single Responsibility Principle (SRP)
```cpp
// Before: one class, three responsibilities.
class ReportGenerator {
public:
    std::vector<double> readData(const std::string& filename);     // responsibility 1: I/O
    double computeAverage(const std::vector<double>& data);         // responsibility 2: computation
    void printReport(double average);                                // responsibility 3: presentation
};

// After: each responsibility is its own class.
class DataReader {
public:
    std::vector<double> readData(const std::string& filename);
};

class StatisticsCalculator {
public:
    double computeAverage(const std::vector<double>& data) const;
};

class ReportPrinter {
public:
    void printReport(double average) const;
};
```
The **Single Responsibility Principle** says a class should have one reason to change. The
"before" `ReportGenerator` changes if the file format changes, if the statistic computed changes,
*or* if the report's wording changes — three unrelated reasons mixed into one class. Splitting it
(the "after" version) means a change to how reports are printed cannot accidentally break how data
is read, and each smaller class is easier to test and to reuse on its own.

## 4. The Open/Closed Principle (OCP)
```cpp
// Violates OCP: adding a new shape means editing this function.
double areaOf(const std::string& shapeType, double dim1, double dim2) {
    if (shapeType == "circle") return 3.14159 * dim1 * dim1;
    else if (shapeType == "rectangle") return dim1 * dim2;
    // adding "triangle" means coming back here and adding another branch
    return 0;
}

// Follows OCP: adding a new shape means adding a new class, no existing code changes.
class Shape {
public:
    virtual double area() const = 0;
    virtual ~Shape() = default;
};
class Circle : public Shape { /* ... override area() ... */ };
class Rectangle : public Shape { /* ... override area() ... */ };
// Adding Triangle : public Shape requires NO changes to any code that already uses Shape*.
```
The **Open/Closed Principle** says a design should be open to extension (new behavior can be
added) but closed to modification (adding that behavior shouldn't require editing existing,
already-tested code). The `if`/`else` chain on a type string violates this directly — exactly the
problem Weeks 8–9's virtual functions and abstract base classes solve: a new `Shape` subclass
plugs into every piece of code that already works with `Shape*`, with zero changes to that code.

## 5. In-Class Exercise
Sketch a UML class diagram (attributes, methods, and the correct arrow type) for the
`Shape`/`Circle`/`Rectangle` hierarchy from Week 9, plus a `ShapeCollection` class that is
composed of a `std::vector<Shape*>`. Then identify one class from any earlier lab or assignment
that violates SRP, and describe (in words, no code required) how you would split it.
