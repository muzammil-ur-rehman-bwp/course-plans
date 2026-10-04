# Course Contents: Object Oriented Programming (C++)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Classes Recap & Encapsulation
- **Topics:** Recap of `class` vs. `struct` from the prerequisite course; access specifiers
  (`public`/`private`/`protected`); why `protected` exists (preview of inheritance); getters and
  setters; `const` member functions and `const`-correctness on accessors.
- **Subtopics/Skills:** auditing a class for which members should be private vs. exposed through
  an accessor; writing a `const` getter and explaining why a setter normally cannot be `const`.
- **Readings:** *C++ Primer* ch. on classes (access control); *C++ How to Program* ch. on classes
  and objects.
- **Software:** g++/clang++, VS Code.

## Week 2 — Constructors in Depth; Destructors
- **Topics:** Default, parameterized, and copy constructors; member initializer lists (and why
  they differ from assignment in the constructor body); destructors and when they run; the Rule
  of Three (if you write a destructor, copy constructor, or copy-assignment operator, you likely
  need all three) introduced as a checklist for later weeks.
- **Subtopics/Skills:** writing a constructor that uses a member initializer list correctly
  (including initializing `const`/reference members, which *must* use one); writing a destructor
  that releases a resource; tracing when a copy constructor is invoked implicitly (pass-by-value,
  return-by-value).
- **Readings:** *C++ Primer* ch. on constructors/copy control; *C++ How to Program* ch. on
  constructors and destructors.
- **Software:** g++/clang++.
- **Quiz 1** this week — Week 1 (encapsulation, access specifiers, `const` member functions).

## Week 3 — Operator Overloading I: Arithmetic & Comparison
- **Topics:** Overloading `+`, `-`, `*`, `==`, `<`, etc. as member functions; return-by-value vs.
  return-by-reference for overloaded operators; overloading compound-assignment operators
  (`+=`); when an operator should be a member vs. a free function (preview of Week 4).
- **Subtopics/Skills:** implementing arithmetic operators for a small value type (e.g.
  `Complex`, `Fraction`) that return a new object rather than mutating an operand; implementing
  `==`/`<` consistently with each other.
- **Readings:** *C++ Primer* ch. on operator overloading (arithmetic/relational operators).
- **Software:** g++/clang++.

## Week 4 — Operator Overloading II: Stream Operators & `friend`
- **Topics:** Why `operator<<`/`operator>>` cannot be ordinary member functions of the class
  being printed (the left-hand operand must be the stream); the `friend` keyword and what access
  it grants; implementing `operator<<(std::ostream&, const T&)` and `operator>>(std::istream&,
  T&)` as `friend` functions.
- **Subtopics/Skills:** writing a correct, chainable `operator<<` that returns `std::ostream&`;
  understanding that `friend` breaks encapsulation deliberately and narrowly, and should be used
  sparingly.
- **Readings:** *C++ Primer* ch. on overloaded operators, `friend` functions; cppreference on
  stream insertion/extraction.
- **Software:** g++/clang++, `<iostream>`.
- **Quiz 2** this week — Weeks 2–3 (constructors/destructors, operator overloading I).
- **Assignment 1 assigned** (encapsulation, constructors/destructors, operator overloading I),
  due start of Week 6.

## Week 5 — Composition ("Has-A" Relationships)
- **Topics:** Objects as data members of another class; construction order (members constructed
  in declaration order, before the enclosing class's constructor body runs) and destruction order
  (reverse); initializing member objects via the member initializer list; composition as the
  default, lower-coupling alternative to inheritance.
- **Subtopics/Skills:** designing a class (e.g. `Car` composed of an `Engine`) where the outer
  class's constructor correctly initializes its member objects; explaining "has-a" vs. the "is-a"
  relationship inheritance will introduce next week.
- **Readings:** *C++ How to Program* ch. on composition/aggregation; *C++ Primer* ch. on class
  design.
- **Software:** g++/clang++.
- **Quiz 3** this week — Weeks 4–5 (stream operators/`friend`, composition).

## Week 6 — Inheritance I: Base/Derived Classes
- **Topics:** `class Derived : public Base`; what a derived class inherits; `protected` members
  (accessible to derived classes, not to outside code); constructor chaining — a derived class's
  constructor must invoke a base-class constructor (explicitly via the initializer list, or
  implicitly via the base's default constructor); destructor call order (derived, then base).
- **Subtopics/Skills:** designing a small 2-level hierarchy (e.g. `Animal` → `Dog`); writing a
  derived constructor that passes arguments to a non-default base constructor via `Base(args)` in
  its initializer list.
- **Readings:** *C++ Primer* ch. on inheritance basics; *C++ How to Program* ch. on inheritance.
- **Software:** g++/clang++.

## Week 7 — Inheritance II: Overriding & Multiple Inheritance
- **Topics:** Overriding (redefining) a base class's member function in a derived class; name
  hiding vs. overriding; the `override` keyword (catching mismatched signatures at compile time);
  multiple inheritance (`class C : public A, public B`) and the diamond problem — briefly, as a
  pitfall to recognize and generally avoid rather than a technique to rely on.
- **Subtopics/Skills:** overriding a method and marking it `override`; explaining, with a
  diagram, why diamond inheritance (two base classes sharing a common ancestor) creates ambiguity
  about which ancestor's data a most-derived object actually has.
- **Readings:** *C++ Primer* ch. on inheritance (overriding); *C++ How to Program* ch. on
  multiple inheritance (survey level).
- **Software:** g++/clang++.
- **Quiz 4** this week — Weeks 6–7 (inheritance I & II).

## Week 8 — Polymorphism I: Virtual Functions; Midterm Review
- **Topics:** Static vs. dynamic binding; the `virtual` keyword and the virtual function table
  (vtable) at a conceptual level; dynamic dispatch through a base-class pointer/reference; virtual
  destructors — *why* a base class with any virtual function (or that is ever deleted
  polymorphically) must have a `virtual` destructor, and what happens (undefined behavior, leaked
  derived resources) if it does not; object slicing — what happens when a derived object is
  assigned or passed **by value** as its base type, and why polymorphism requires pointers or
  references, not by-value objects; midterm review session (Weeks 1–8).
- **Subtopics/Skills:** writing a base class with virtual methods and a virtual destructor;
  demonstrating slicing with a deliberately-by-value example and fixing it with a reference or
  pointer; practice problems for the midterm.
- **Readings:** *C++ Primer* ch. on virtual functions; *C++ How to Program* ch. on polymorphism
  (virtual functions, slicing).
- **Software:** g++/clang++.
- **Assignment 2 assigned** (stream operators/`friend`, composition, inheritance I & II), due
  start of Week 10.

## Week 9 — Midterm Exam; Polymorphism II: Abstract Classes
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: pure virtual functions (`virtual void
  f() = 0;`); abstract base classes (a class with at least one pure virtual function cannot be
  instantiated); using an abstract base class to define an interface-by-convention in C++ (no
  separate `interface` keyword, unlike Java/C#); designing a small polymorphic hierarchy (e.g.
  `Shape` with derived `Circle`, `Rectangle`) driven entirely through base-class pointers.
- **Readings:** *C++ Primer* ch. on abstract base classes; *C++ How to Program* ch. on abstract
  classes and interfaces.
- **Software:** g++/clang++.
- **Capstone project introduced** (proposal due Week 11).

## Week 10 — Templates I: Function Templates
- **Topics:** Motivation (avoiding near-duplicate overloads for each type); `template <typename
  T>` function syntax; template argument deduction from call-site arguments; explicit
  instantiation when deduction is ambiguous; constraints at an intuitive level (a template
  function only compiles for types that support the operations it uses).
- **Subtopics/Skills:** writing a generic `max`/`swap`/`printAll` function template; tracing which
  concrete type the compiler instantiates for a given call.
- **Readings:** *C++ Primer* ch. on generic programming/templates (function templates).
- **Software:** g++/clang++.

## Week 11 — Templates II: Class Templates
- **Topics:** `template <typename T> class`; writing a generic container (e.g. `Stack<T>` backed
  by `std::vector<T>`, or a `Pair<T, U>` with two type parameters); member function definitions
  outside the class body for a template; instantiating the same template for multiple types in one
  program.
- **Subtopics/Skills:** implementing a generic `Stack<T>` with `push`/`pop`/`top`/`empty`; using
  it with at least two different element types to show genuine type independence.
- **Readings:** *C++ Primer* ch. on class templates.
- **Software:** g++/clang++.
- **Quiz 5** this week — Weeks 9–10 (abstract classes, function templates).
- **Assignment 3 assigned** (abstract classes, function/class templates), due start of Week 13.

## Week 12 — Exception Handling
- **Topics:** `throw`, `try`, `catch`; stack unwinding when an exception propagates; the standard
  exception hierarchy (`std::exception` and friends — `std::runtime_error`,
  `std::out_of_range`, `std::invalid_argument`); writing a custom exception class derived from
  `std::exception` (overriding `what()`); catching by reference (and why catching by value risks
  slicing an exception object, tying back to Week 8); multiple `catch` clauses and catch-all
  (`catch (...)`).
- **Subtopics/Skills:** writing a function that validates input and `throw`s an appropriate
  standard or custom exception; writing a `try`/`catch` block with ordered, specific-to-general
  `catch` clauses.
- **Readings:** *C++ Primer* ch. on exception handling; cppreference on `<stdexcept>`.
- **Software:** g++/clang++, `<stdexcept>`, `<exception>`.

## Week 13 — Introduction to the STL
- **Topics:** `std::vector<T>` as a growable, type-safe array (bridging directly to Weeks 10–11's
  templates); `std::map<K, V>` as an associative container; iterators as a uniform way to traverse
  any container (`begin()`/`end()`, range-`for`); a brief look at how `std::vector` and `std::map`
  are themselves class templates students now understand the shape of.
- **Subtopics/Skills:** replacing a hand-rolled dynamic array or lookup structure with
  `std::vector`/`std::map`; iterating a container with both a range-`for` loop and an explicit
  iterator.
- **Readings:** *C++ Primer* ch. on the standard library containers and generic algorithms;
  cppreference on `std::vector`, `std::map`, iterators.
- **Software:** g++/clang++, `<vector>`, `<map>`.
- **Quiz 6** this week — Weeks 11–12 (class templates, exception handling).
- **Assignment 4 assigned** (exception handling, intro to STL), due start of Week 15.

## Week 14 — Software Design: UML, Composition vs. Inheritance, SOLID
- **Topics:** Reading and drawing a basic UML class diagram (classes, attributes, methods,
  composition/aggregation vs. inheritance arrows); revisiting "is-a" (inheritance) vs. "has-a"
  (composition) as a design decision, not just a syntax choice, with guidance to prefer
  composition when there is no true is-a relationship; an undergraduate-level introduction to two
  SOLID principles — the Single Responsibility Principle and the Open/Closed Principle — with
  small C++ before/after examples.
- **Subtopics/Skills:** drawing a UML class diagram for a small hierarchy designed earlier in the
  course; critiquing a class that does "too much" and refactoring it to follow single
  responsibility; recognizing an Open/Closed violation (a `switch` on type that must be edited for
  every new case) and how polymorphism (Weeks 8–9) resolves it.
- **Readings:** *C++ How to Program* ch. on object-oriented design/UML; supplementary notes on
  SOLID (undergraduate-level treatment).
- **Software:** g++/clang++; any diagramming tool (or pen and paper) for UML.

## Week 15 — RAII & Smart Pointers; Debugging/Testing OOP Code
- **Topics:** Resource Acquisition Is Initialization (RAII) as the general C++ pattern behind
  destructors already used since Week 2; the problem with raw owning pointers (leaks, double
  frees, exception-unsafety); `std::unique_ptr<T>` for exclusive ownership; `std::shared_ptr<T>`
  for shared ownership (reference counting, at a conceptual level); `make_unique`/`make_shared`;
  basic debugging/testing practices for OOP code (using a debugger to step through a constructor/
  destructor pair, writing small hand-written test cases for a class's public interface).
- **Subtopics/Skills:** replacing a raw `new`/`delete` pair in an existing class with
  `std::unique_ptr`; choosing between `unique_ptr` and `shared_ptr` for a given ownership
  scenario; writing a few test cases that exercise a class's constructors, overloaded operators,
  and virtual methods.
- **Readings:** *C++ Primer* ch. on smart pointers/dynamic memory; cppreference on
  `std::unique_ptr`, `std::shared_ptr`.
- **Software:** g++/clang++, `<memory>`, gdb or IDE debugger.

## Week 16 — Capstone Presentations, Course Review
- **Topics:** Student capstone project presentations; recap of the course map (encapsulation →
  constructors/destructors → operator overloading → composition → inheritance → polymorphism →
  templates → exceptions → STL → design → RAII/smart pointers); brief look ahead to Data
  Structures & Algorithms and other follow-on courses.
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Capstone Project (introduced Week 9, proposal Week 11, final Week 16)
Students (individually or in pairs) design and build a C++ console application that models a
small domain using a polymorphic class hierarchy, and that integrates inheritance, polymorphism
(virtual functions, an abstract base class), at least one template or STL container, and exception
handling for invalid input or invalid operations end-to-end. Example topics: a shape hierarchy
with an area/perimeter calculator, a small inventory system with polymorphic item types (e.g.
physical vs. digital goods), or a mini banking system with an account-type hierarchy (e.g.
checking vs. savings, each with different interest/fee behavior) — each driven through base-class
pointers or references, each storing its objects in an STL container, and each handling at least
one realistic error condition (invalid amount, missing record) by throwing and catching an
exception.
