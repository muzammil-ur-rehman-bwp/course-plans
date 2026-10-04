# Week 6 Lecture Plan — Object Oriented Programming (C++)
## Topic: Inheritance I — Base/Derived Classes

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the syntax `class Derived : public Base` and what a derived class inherits. (*Remember*)
2. Explain the role of `protected` members and how constructor chaining works between base and derived classes. (*Understand*)
3. Apply inheritance to design a small two-level hierarchy with correct constructor chaining. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | "Is-a" vs. "has-a" | Contrast with Week 5's composition; when inheritance is the right tool |
| 0:20–0:45 | Base/derived syntax | `class Dog : public Animal`; what members a `Dog` inherits |
| 0:45–1:15 | `protected` revisited | Now meaningfully different from `private` — accessible to `Dog`, not to outside code |
| 1:15–1:45 | Constructor chaining | Live-coded: derived constructor calling a non-default base constructor via the initializer list |
| 1:45–1:55 | Destructor order | Derived destructor runs first, then base destructor |
| 1:55–2:00 | Formative check | — |

### Materials/Equipment
- Slides: "Inheritance I"
- Live-coding environment (VS Code + terminal)

### Formative Check (in-class)
If `Base` has no default constructor, why must every `Derived` constructor explicitly call a
specific `Base` constructor in its own initializer list?

### Link to Lab/Assessment
Lab 6: Inheritance I exercises (see `lab-manuals/lab-06.md`).
