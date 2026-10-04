# Week 6 — Lecture Content: Inheritance I — Base/Derived Classes

## 1. "Is-A": The Relationship Inheritance Models
```cpp
class Animal { /* ... */ };
class Dog : public Animal { /* ... */ };   // a Dog IS-A Animal
```
Where composition (Week 5) models "has-a", inheritance models **"is-a"**: a `Dog` is a more
specific *kind of* `Animal` — every `Dog` genuinely is an `Animal`, with all of `Animal`'s
properties plus some of its own. Reach for inheritance only when this is actually true of your
domain; a common design mistake is using inheritance where composition ("has-a") was the honest
relationship (more on this trade-off in Week 14).

## 2. Base and Derived Classes
```cpp
class Animal {
public:
    Animal(const std::string& name) : name_(name) {}

    std::string getName() const { return name_; }

    void eat() const { std::cout << name_ << " is eating.\n"; }

protected:
    std::string name_;   // accessible to Dog (derived), not to outside code
};

class Dog : public Animal {
public:
    Dog(const std::string& name, const std::string& breed)
        : Animal(name), breed_(breed) {}      // constructor chaining — see section 4

    void bark() const {
        std::cout << name_ << " (" << breed_ << ") says Woof!\n";  // name_ visible: protected
    }

private:
    std::string breed_;
};
```
`class Dog : public Animal` makes `Dog` publicly inherit from `Animal`: every `public` and
`protected` member of `Animal` becomes a member of `Dog` too (with the same access level), and
`Dog` adds its own members (`breed_`, `bark()`) on top. A `Dog` object can be used anywhere an
`Animal` is expected (through a pointer or reference — more on this in Week 8).

## 3. `protected`, Now It Matters
```cpp
void brokenExample(Dog& d) {
    // d.name_ = "Rex";   // ERROR: name_ is protected, NOT accessible from outside the class hierarchy
}
```
`protected` members are accessible inside the base class itself **and** inside any derived
class's own member functions (as `name_` is inside `Dog::bark()`), but still inaccessible to
ordinary outside code — exactly the middle ground promised back in Week 1. Prefer `private` with
protected *accessor* methods unless a derived class genuinely needs direct data access; overusing
`protected` data weakens encapsulation for every future derived class, not just the one you have
in mind today.

## 4. Constructor Chaining
```cpp
class Dog : public Animal {
public:
    Dog(const std::string& name, const std::string& breed)
        : Animal(name),     // explicitly calls Animal's constructor with `name`
          breed_(breed) {}
    // ...
};
```
A derived class's constructor **must** arrange for a base-class constructor to run — explicitly,
by naming it in the initializer list (`Animal(name)`), or implicitly, if `Animal` has an
accessible default constructor and the initializer list doesn't mention it. If `Animal` has no
default constructor and `Dog`'s constructor doesn't call one of `Animal`'s constructors
explicitly, the code does not compile. The base part of the object is always fully constructed
*before* the derived part — `breed_` is initialized only after `Animal(name)` has completed.

## 5. Destructor Order: Exactly Reversed From Construction
```cpp
class Animal {
public:
    ~Animal() { std::cout << "Animal destroyed.\n"; }
    // ...
};
class Dog : public Animal {
public:
    ~Dog() { std::cout << "Dog destroyed.\n"; }
    // ...
};

// Output when a Dog is destroyed:
// Dog destroyed.
// Animal destroyed.
```
Just like composed member objects (Week 5), the derived part is destroyed first, then the base
part — the exact reverse of construction order (base first, then derived).

## 6. In-Class Exercise
Design a base class `Vehicle` (with a `protected` `std::string model_` and a constructor taking
it) and a derived class `Motorcycle` that adds its own `int engineCC_` member. Write
`Motorcycle`'s constructor with correct constructor chaining, and a `describe() const` method
that uses both `model_` (inherited, `protected`) and `engineCC_` (its own).
