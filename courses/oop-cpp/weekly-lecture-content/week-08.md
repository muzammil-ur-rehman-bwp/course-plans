# Week 8 — Lecture Content: Polymorphism I — Virtual Functions; Midterm Review

## 1. The Problem: Static Binding Through a Base Pointer
```cpp
class Animal {
public:
    Animal(std::string name) : name_(name) {}
    void speak() const { std::cout << name_ << " makes a sound.\n"; }   // NOT virtual
protected:
    std::string name_;
};

class Dog : public Animal {
public:
    Dog(std::string name) : Animal(name) {}
    void speak() const { std::cout << name_ << " says Woof!\n"; }
};

Animal* a = new Dog("Rex");
a->speak();   // prints "Rex makes a sound." — NOT "Rex says Woof!" — wrong, and surprising
delete a;
```
Without `virtual`, which `speak()` runs is decided at compile time from the **static type of the
pointer** (`Animal*`), not the actual object it points to (`Dog`). This is almost never what you
want when working through base-class pointers/references — it defeats the entire point of having
a `Dog`-specific `speak()`.

## 2. `virtual` and Dynamic Dispatch
```cpp
class Animal {
public:
    Animal(std::string name) : name_(name) {}
    virtual void speak() const { std::cout << name_ << " makes a sound.\n"; }   // now virtual
protected:
    std::string name_;
};

class Dog : public Animal {
public:
    Dog(std::string name) : Animal(name) {}
    void speak() const override { std::cout << name_ << " says Woof!\n"; }   // overrides correctly
};

Animal* a = new Dog("Rex");
a->speak();   // now prints "Rex says Woof!" — the ACTUAL (dynamic) type decides
delete a;
```
Marking `speak()` `virtual` in the base class makes the call resolve at runtime based on the
object's actual (dynamic) type — this is **dynamic dispatch**. Conceptually, each class with
virtual functions has a hidden table of function pointers (the **vtable**); a virtual call looks
up the right function in the actual object's vtable instead of hard-coding which version to call
at compile time. Once a function is declared `virtual` in a base class, every override in every
derived class is virtual too, automatically.

## 3. Virtual Destructors
```cpp
class Animal {
public:
    Animal(std::string name) : name_(name) {}
    virtual ~Animal() { std::cout << "Animal destroyed.\n"; }   // virtual destructor
    virtual void speak() const { /* ... */ }
protected:
    std::string name_;
};

class Dog : public Animal {
public:
    Dog(std::string name) : Animal(name), tag_(new int[10]) {}
    ~Dog() override { delete[] tag_; std::cout << "Dog destroyed.\n"; }
private:
    int* tag_;
};

Animal* a = new Dog("Rex");
delete a;   // with a VIRTUAL ~Animal(): correctly calls ~Dog() first, then ~Animal() — tag_ freed
```
**Rule:** if a class has *any* virtual function, or if objects of a derived type will ever be
`delete`d through a base-class pointer, the base class's destructor **must** be `virtual`. Without
`virtual`, `delete a;` above would call only `~Animal()` — `~Dog()` would never run, `tag_` would
leak, and in general this is undefined behavior for polymorphic deletion. This is one of the most
important rules in all of C++ OOP: **a polymorphic base class needs a virtual destructor**, full
stop, even if that destructor's body is empty.

## 4. Object Slicing
```cpp
class Animal {
public:
    Animal(std::string name) : name_(name) {}
    virtual void speak() const { std::cout << name_ << " makes a sound.\n"; }
protected:
    std::string name_;
};

class Dog : public Animal {
public:
    Dog(std::string name, std::string breed) : Animal(name), breed_(breed) {}
    void speak() const override { std::cout << name_ << " (" << breed_ << ") says Woof!\n"; }
private:
    std::string breed_;
};

void describe(Animal a) {   // BY VALUE — takes an Animal, not a reference/pointer
    a.speak();               // always calls Animal::speak — "sliced" to just the Animal part
}

Dog d("Rex", "Labrador");
describe(d);   // prints "Rex makes a sound." — breed_ is GONE, and Dog::speak never runs
```
Passing (or assigning) a `Dog` where an `Animal` is expected **by value** copies only the
`Animal` portion of the object — the `Dog`-specific parts (`breed_`, the overridden `speak()`)
are discarded. This is **object slicing**, and it silently defeats polymorphism even though the
code compiles cleanly. The fix is always the same: **polymorphism requires pointers or
references**, never plain by-value objects/parameters:
```cpp
void describe(const Animal& a) {   // reference — no slicing, dynamic dispatch works
    a.speak();                      // correctly calls Dog::speak via the vtable
}
```

## 5. Midterm Review: Topics Covered (Weeks 1–8)
- Encapsulation, access specifiers, `const` member functions (Week 1)
- Constructors, member initializer lists, destructors, Rule of Three (Week 2)
- Operator overloading: arithmetic/comparison as members (Week 3); streams via `friend` (Week 4)
- Composition — "has-a" (Week 5)
- Inheritance — base/derived, `protected`, constructor chaining (Week 6); overriding, multiple
  inheritance/diamond problem (Week 7)
- Virtual functions, dynamic dispatch, virtual destructors, object slicing (Week 8)

## 6. In-Class Exercise
Given the `Animal`/`Dog` hierarchy above, write a function `void processAll(const
std::vector<Animal*>& animals)` that calls `speak()` on each — correctly using dynamic dispatch —
and verify that mixing `Animal*` and `Dog*` pointers in the vector still calls each object's own
`speak()`. Then work through 3–4 midterm-style practice problems as a class.
