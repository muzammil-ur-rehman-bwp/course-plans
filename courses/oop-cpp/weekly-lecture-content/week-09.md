# Week 9 — Lecture Content: Polymorphism II — Abstract Classes

> After today's midterm, we complete the polymorphism picture started in Week 8 by introducing
> pure virtual functions and abstract base classes — the C++ way of defining "this hierarchy must
> provide this behavior" without providing a default implementation.

## 1. Pure Virtual Functions
```cpp
class Shape {
public:
    virtual double area() const = 0;        // PURE virtual: no body, "= 0"
    virtual void draw() const = 0;          // every derived class MUST provide one
    virtual ~Shape() = default;              // still needs a virtual destructor (Week 8)
};
```
A **pure virtual function** is declared with `= 0` instead of a body. It says: "every concrete
(instantiable) class in this hierarchy must supply its own implementation of this function" — the
base class deliberately provides none, because there usually isn't a sensible default (what would
a generic `Shape`'s `area()` even return?).

## 2. Abstract Base Classes
```cpp
class Shape {
public:
    virtual double area() const = 0;
    virtual ~Shape() = default;
};

// Shape s;   // ERROR: cannot instantiate an abstract class

class Circle : public Shape {
public:
    Circle(double radius) : radius_(radius) {}
    double area() const override { return 3.14159 * radius_ * radius_; }
private:
    double radius_;
};

class Rectangle : public Shape {
public:
    Rectangle(double w, double h) : width_(w), height_(h) {}
    double area() const override { return width_ * height_; }
private:
    double width_, height_;
};

Circle c(2.0);
Rectangle r(3.0, 4.0);
Shape* shapes[] = { &c, &r };
for (Shape* s : shapes) {
    std::cout << "Area: " << s->area() << "\n";   // dynamic dispatch picks the right area()
}
```
A class with at least one pure virtual function is **abstract**: the compiler refuses to let you
create an object of that type directly (`Shape s;` fails to compile), because such an object
would be missing a required piece of behavior. `Circle` and `Rectangle` are **concrete** — they
override every pure virtual function they inherit, so they can be instantiated, and each can be
used polymorphically through a `Shape*`/`Shape&`.

## 3. Interfaces-by-Convention
```cpp
class Printable {
public:
    virtual void print() const = 0;    // every function pure virtual
    virtual ~Printable() = default;    // still needed: see Week 8
};
```
Languages like Java or C# have a dedicated `interface` keyword — a type that defines *only*
method signatures, with no implementation or data. C++ has no such keyword; instead, the
convention is an abstract base class whose member functions are **all** pure virtual (as
`Printable` above), with no data members of its own. Any class that inherits from `Printable` and
implements `print()` can be used anywhere a `Printable&`/`Printable*` is expected — functionally
equivalent to implementing an interface, just expressed through the same `class`/pure-virtual
mechanism used for ordinary abstract base classes.

## 4. Mixing Default and Pure Virtual Behavior
```cpp
class Shape {
public:
    virtual double area() const = 0;                 // must override
    virtual void describe() const {                   // has a sensible default
        std::cout << "A shape with area " << area() << "\n";
    }
    virtual ~Shape() = default;
};
```
An abstract class is not required to make *every* function pure virtual — it can mix pure virtual
functions (no sensible default; must override) with ordinary virtual functions that have a
default implementation derived classes may optionally override. `describe()` above even calls
`area()` polymorphically — at the point `describe()` runs on a `Circle`, `area()` dispatches to
`Circle::area()`, even though `describe()` is defined in `Shape`.

## 5. In-Class Exercise
Add a third derived class `Triangle` (constructed from base and height) to the `Shape` hierarchy
above, overriding `area()` correctly. Build a `std::vector<Shape*>` containing a `Circle`, a
`Rectangle`, and a `Triangle`, and print each one's `area()` through a loop over `Shape*` — this
is the pattern the capstone project (introduced this week) will build on.
