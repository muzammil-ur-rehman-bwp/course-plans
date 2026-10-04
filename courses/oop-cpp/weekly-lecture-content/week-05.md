# Week 5 — Lecture Content: Composition ("Has-A" Relationships)

## 1. "Has-A": An Object as a Data Member
```cpp
class Engine {
public:
    Engine(int horsepower) : horsepower_(horsepower) {}
    int getHorsepower() const { return horsepower_; }
    void start() const { std::cout << "Engine with " << horsepower_ << "hp starting.\n"; }

private:
    int horsepower_;
};

class Car {
public:
    Car(const std::string& model, int horsepower)
        : model_(model), engine_(horsepower) {}   // Engine constructed via initializer list

    void start() const {
        std::cout << model_ << ": ";
        engine_.start();
    }

private:
    std::string model_;
    Engine engine_;   // Car HAS-A Engine
};
```
`Car` is **composed of** an `Engine` — `Engine engine_` is a regular data member whose type
happens to be a class. This is the **"has-a"** relationship: a `Car` *has* an `Engine`, it is not
*itself* a kind of `Engine`. Composition is the right tool whenever one object is a genuine part
of another, as opposed to being a more specific kind of something (which is "is-a", the subject
of inheritance starting next week).

## 2. Construction Order
```cpp
class Car {
public:
    Car(const std::string& model, int horsepower)
        : model_(model), engine_(horsepower) {
        std::cout << "Car constructor body running.\n";
    }

private:
    std::string model_;   // declared first -> constructed first
    Engine engine_;       // declared second -> constructed second
};
```
Member objects are constructed in the order they are **declared in the class** (not the order
written in the initializer list), and all of them finish constructing *before* the enclosing
class's own constructor body starts running. So for the `Car` above: `model_`'s constructor runs,
then `engine_`'s constructor runs (printing `"Engine with ...hp starting."` only if `start()`
were called there — the constructor itself just builds the `Engine`), and only then does `"Car
constructor body running."` print.

## 3. Destruction Order: Exactly Reversed
```cpp
// When a Car is destroyed:
// 1. Car's own destructor body runs (if one is defined)
// 2. engine_'s destructor runs (last-declared member, destroyed first)
// 3. model_'s destructor runs (first-declared member, destroyed last)
```
Destruction always undoes construction in reverse: the enclosing class's destructor body runs
first, then its members are destroyed in **reverse declaration order**. This matters whenever a
later member depends on an earlier one being still-valid during destruction — declare members in
an order that respects any such dependency.

## 4. Composition With Multiple Member Objects
```cpp
class Engine { /* ... as above ... */ };

class WheelSet {
public:
    WheelSet(int count) : count_(count) {}
    int getCount() const { return count_; }

private:
    int count_;
};

class Car {
public:
    Car(const std::string& model, int horsepower, int wheelCount)
        : model_(model), engine_(horsepower), wheels_(wheelCount) {}

    void describe() const {
        std::cout << model_ << ": " << engine_.getHorsepower() << "hp, "
                  << wheels_.getCount() << " wheels\n";
    }

private:
    std::string model_;
    Engine engine_;
    WheelSet wheels_;
};
```
A class can be composed of as many member objects as its design calls for — each is initialized
in the initializer list exactly like a primitive member, just with constructor arguments instead
of a plain value.

## 5. Composition vs. Just Using a Pointer/Reference Member
```cpp
class CarByPointer {
public:
    CarByPointer(Engine* engine) : engine_(engine) {}  // Car does NOT own this Engine
private:
    Engine* engine_;   // a pointer member: aggregation, not full ownership
};
```
What this week covers is specifically **ownership composition**: the member object's lifetime is
tied to the owning object's lifetime (it's constructed with it, destroyed with it). Holding a
pointer/reference to an object owned *elsewhere* is a related but different relationship
(sometimes called aggregation) — the `Car` doesn't own that `Engine`'s lifetime, and must not
`delete` it in its own destructor unless it explicitly took ownership.

## 6. In-Class Exercise
Design a class `Library` composed of a `std::string name_` and an `Engine`-style member of your
own design (e.g. a small `Catalog` class tracking a book count). Write `Library`'s constructor
with a correct member initializer list, and add a `describe() const` method that reports both
members' state.
