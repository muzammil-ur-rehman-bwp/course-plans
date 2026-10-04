# Week 16 — Lecture Content: Capstone Presentations; Course Review

> This week has no new technical content — it is dedicated to capstone presentations and a
> structured recap of the semester. The material below supports the review segment.

## 1. The Course Map
```
Week 1  Encapsulation (access specifiers, const member functions)
Week 2  Constructors/destructors in depth, member initializer lists, Rule of Three
Week 3  Operator overloading I (arithmetic/comparison, member functions)
Week 4  Operator overloading II (stream operators, friend)
Week 5  Composition ("has-a")
Week 6  Inheritance I (base/derived, protected, constructor chaining)
Week 7  Inheritance II (overriding, override, multiple inheritance/diamond problem)
Week 8  Polymorphism I (virtual, dynamic dispatch, virtual destructors, object slicing)
Week 9  Polymorphism II (pure virtual functions, abstract base classes)
Week 10 Templates I (function templates)
Week 11 Templates II (class templates)
Week 12 Exception handling (try/catch/throw, custom exceptions)
Week 13 Introduction to the STL (std::vector, std::map, iterators)
Week 14 Software design (UML, composition vs. inheritance, SOLID)
Week 15 RAII and smart pointers (unique_ptr, shared_ptr)
Week 16 Capstone presentations; course review
```
Each week built directly on earlier ones: operator overloading (Weeks 3–4) used constructors
(Week 2); inheritance (Weeks 6–7) and polymorphism (Weeks 8–9) used encapsulation's `protected`
(Week 1); templates (Weeks 10–11) generalized the classes built throughout; the STL (Week 13) is
templates already in the standard library; exceptions (Week 12) and smart pointers (Week 15) both
rest on RAII, which traces back to Week 2's destructors.

## 2. Five Ideas Worth Re-Stating Precisely, One Last Time
1. **Virtual destructors:** any base class used polymorphically (deleted through a base pointer)
   must have a `virtual` destructor, or derived resources leak and the behavior is undefined.
2. **Object slicing:** passing/assigning a derived object *by value* as its base type discards
   the derived part — polymorphism requires pointers or references, never by-value objects.
3. **The Rule of Three:** a custom destructor, copy constructor, or copy-assignment operator
   usually implies needing all three, to avoid double frees/dangling pointers on a raw owning
   pointer — solved more robustly by smart pointers (Week 15).
4. **Composition vs. inheritance:** default to composition ("has-a"); use inheritance only for a
   genuine "is-a" relationship, not merely to reuse code.
5. **Abstract classes as interfaces:** C++ has no `interface` keyword; an all-pure-virtual
   abstract base class fills that role by convention.

## 3. Looking Ahead
The classes, hierarchies, templates, and containers built this semester are the foundation for a
follow-on Data Structures & Algorithms course, which formalizes the complexity analysis this
course only gestured at (Week 14's design discussion) and builds custom data structures (trees,
graphs, hash tables) using exactly the class-design tools — constructors, operator overloading,
templates, RAII — covered here.

## 4. In-Class Activity
During Q&A after each capstone presentation, ask the presenter to identify, in their own
submitted code, one concrete example each of: a virtual function call that required dynamic
dispatch, a place object slicing would have occurred had a parameter been by-value instead of by
reference/pointer, and the `catch` clause that handles their riskiest realistic error condition.
