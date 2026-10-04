# Week 1 — Lecture Content: Classes Recap & Encapsulation

> This course picks up exactly where Programming Fundamentals (C++) left off. That course's
> Week 13 gave you private data, a constructor, and public accessor/mutator methods — on purpose,
> stopping short of inheritance and polymorphism. This week we tighten up encapsulation; from
> Week 2 onward we build everything that course deliberately left out.

## 1. Access Specifiers: A Quick Recap
```cpp
class Account {
public:
    Account(double startingBalance) : balance(startingBalance) {}
    double getBalance() const { return balance; }

private:
    double balance;   // not accessible outside the class
};
```
`public` members form the class's interface — what other code is allowed to use. `private`
members are implementation detail, visible only inside the class's own member functions. A
well-designed class exposes the smallest `public` interface that lets callers do what they
legitimately need, and keeps everything else `private`.

## 2. `protected`: A Third Level, for Inheritance
```cpp
class Shape {
protected:
    double area;   // NOT accessible from outside Shape or its derived classes...
                    // ...but WILL be accessible from inside a derived class (Week 6)

private:
    std::string label;   // accessible only inside Shape itself, even to derived classes
};
```
`protected` is `private` to the outside world, but visible to derived classes. It has no effect
on its own until a class is actually inherited from (Week 6) — this week, just remember that
`protected` exists as a middle ground, and that most data should still default to `private`
unless there is a concrete reason derived classes need direct access to it.

## 3. Getters and Setters
```cpp
class Temperature {
public:
    Temperature(double celsius) { setCelsius(celsius); }

    double getCelsius() const { return celsius_; }

    void setCelsius(double value) {
        if (value < -273.15) {
            celsius_ = -273.15;   // clamp to absolute zero instead of storing nonsense
        } else {
            celsius_ = value;
        }
    }

private:
    double celsius_;
};
```
A **getter** reads a private member; a **setter** writes one, and is the natural place to
validate a new value before it is stored. Not every private member needs both — some are
read-only from outside the class (only a getter), and some should never be settable after
construction at all (no setter, set once in the constructor).

## 4. `const` Member Functions and `const`-Correctness
```cpp
double area(const Temperature& t) {
    return t.getCelsius();   // only compiles if getCelsius() is declared const
}
```
A member function marked `const` (as `getCelsius()` is above) promises it will not modify the
object it is called on. This matters for more than documentation: a `const Temperature&`
parameter can *only* call `const` member functions on it — the compiler rejects an attempt to
call a non-`const` method through a `const` reference. Marking every method that doesn't modify
state as `const` is called `const`-correctness, and it lets the compiler catch accidental
mutations for you.

## 5. Refactoring a Poorly-Encapsulated Class
```cpp
// Before: no encapsulation at all.
struct RawPoint { double x, y; };   // any code anywhere can set x/y to anything

// After: encapsulated, validated, const-correct.
class Point {
public:
    Point(double x, double y) : x_(x), y_(y) {}

    double getX() const { return x_; }
    double getY() const { return y_; }

    void setX(double x) { x_ = x; }
    void setY(double y) { y_ = y; }

private:
    double x_, y_;
};
```
For a simple geometric point, validation may be minimal — but the *shape* of the class (private
data, a constructor, `const` getters, setters that are the only path to mutation) is the pattern
you will reuse and extend for every class this semester, starting with constructors in depth next
week.

## 6. In-Class Exercise
Take the following class and refactor it so that `balance` is private, is only modifiable through
a validated `deposit`/`withdraw` pair of methods, and so that every method that does not change
`balance` is marked `const`:
```cpp
class Account {
public:
    double balance;
};
```
