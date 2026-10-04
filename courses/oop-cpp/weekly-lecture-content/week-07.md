# Week 7 — Lecture Content: Inheritance II — Overriding & Multiple Inheritance

## 1. Overriding a Member Function
```cpp
class Animal {
public:
    Animal(const std::string& name) : name_(name) {}
    void speak() const { std::cout << name_ << " makes a sound.\n"; }
protected:
    std::string name_;
};

class Dog : public Animal {
public:
    Dog(const std::string& name) : Animal(name) {}
    void speak() const { std::cout << name_ << " says Woof!\n"; }   // overrides Animal::speak
};

Dog d("Rex");
d.speak();   // "Rex says Woof!" — Dog::speak is used, since d's static type is Dog
```
A derived class can define a member function with the **same name and signature** as one in its
base class — this redefines (overrides) that behavior for the derived class. Called directly on
a `Dog` object, as above, the derived version is used. What overriding does *not* yet give you is
dynamic dispatch through a *base-class* pointer/reference — that requires `virtual`, which is
next week's topic; without it, which version runs is decided by the pointer/reference's static
type, not the object's actual type, which is rarely what you want for polymorphic behavior.

## 2. The `override` Keyword
```cpp
class Dog : public Animal {
public:
    Dog(const std::string& name) : Animal(name) {}
    void speak() const override { std::cout << name_ << " says Woof!\n"; }
    //                    ^^^^^^^^ tells the compiler "this must override a base method"
};
```
Writing `override` after a member function's parameter list tells the compiler to verify that a
base-class function with the exact same signature exists. Without it, a small typo — a missing
`const`, a mismatched parameter type, or a misspelled name — silently creates an unrelated new
function instead of overriding anything, and the bug is easy to miss until behavior looks wrong
at runtime. Marking every intended override with `override` turns that mistake into a compile
error instead. (`override` becomes essential, not just good style, once `virtual` is introduced
in Week 8 — always use it from here on.)

## 3. Name Hiding
```cpp
class Base {
public:
    void greet() const { std::cout << "Hello from Base\n"; }
    void greet(int times) const { for (int i = 0; i < times; ++i) greet(); }
};

class Derived : public Base {
public:
    void greet() const { std::cout << "Hi from Derived\n"; }   // hides BOTH Base::greet overloads
};

Derived d;
d.greet();        // "Hi from Derived" — fine
// d.greet(3);    // ERROR: Base::greet(int) is hidden, not inherited visibly, by Derived::greet()
d.Base::greet(3);  // works: explicitly qualify to reach the hidden overload
```
Defining a member function in a derived class with the same *name* as a base member — even with a
different signature — hides **all** base overloads of that name from ordinary lookup on the
derived type, a surprising trap called **name hiding**. This is different from overriding (which
needs a matching signature); name hiding is usually accidental and a sign the derived class
should either add a differently-named method or explicitly bring the base overloads back with a
`using Base::greet;` declaration.

## 4. Multiple Inheritance
```cpp
class Flyable {
public:
    void fly() const { std::cout << "Flying.\n"; }
};

class Swimmable {
public:
    void swim() const { std::cout << "Swimming.\n"; }
};

class Duck : public Flyable, public Swimmable {
    // Duck IS-A Flyable AND IS-A Swimmable
};

Duck d;
d.fly();
d.swim();
```
C++ (unlike Java or C#) allows a class to inherit from more than one base class directly. This is
occasionally useful for combining independent capabilities (as with `Flyable`/`Swimmable` above,
which share no common ancestor and so create no ambiguity), but it should be used sparingly and
deliberately — the pitfall below is why many style guides restrict it further.

## 5. The Diamond Problem
```cpp
class Animal { protected: std::string name_; public: Animal(std::string n) : name_(n) {} };
class Flyable : public Animal { /* ... */ };
class Swimmable : public Animal { /* ... */ };
class Duck : public Flyable, public Swimmable { /* ... */ };
//                   Animal
//                  /      \
//            Flyable      Swimmable
//                  \      /
//                   Duck
```
Here `Flyable` and `Swimmable` **both** inherit from `Animal` — so a `Duck` object, by default,
contains **two separate copies** of `Animal` (one via `Flyable`, one via `Swimmable`), each with
its own `name_`. Code that tries to access `duck.name_` or call an `Animal` method on a `Duck` is
ambiguous: the compiler cannot tell which copy you mean, and refuses to compile without explicit
qualification (`duck.Flyable::name_`). C++ offers **virtual inheritance** (`class Flyable :
public virtual Animal`) to force a single shared `Animal` instead — a real fix, but subtle enough
that this course treats the diamond problem primarily as a pitfall to **recognize and generally
avoid** (e.g. by preferring composition or single inheritance with interfaces, Week 9) rather
than a technique to build designs around.

## 6. In-Class Exercise
Given `Flyable` and `Swimmable` as above (both *without* a common `Animal` base, to avoid the
diamond), define `class Duck : public Flyable, public Swimmable` and call both `fly()` and
`swim()` on a `Duck` object. Then, as a thought exercise (no code required), sketch what would
change if both `Flyable` and `Swimmable` inherited from a shared `Animal` base.
