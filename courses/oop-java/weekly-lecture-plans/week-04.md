# Week 4 Lecture Plan — Object Oriented Programming (Java)
## Topic: Static vs. Instance Members

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall that `main` has always been `static`. (*Remember*)
2. Explain the difference between a per-object instance field and a per-class static field. (*Understand*)
3. Apply static fields, static methods, and static initialization to build a utility class. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 3's composed classes |
| 0:10–0:35 | Instance vs. static fields | A shared counter field vs. a per-object field, side by side |
| 0:35–1:00 | Static methods | No `this`, called on the class; what a static method may and may not access |
| 1:00–1:25 | Static initialization | Static field initializers and static initializer blocks; when each runs (class-loading time) |
| 1:25–1:50 | Utility classes | A class of only `static` methods with a `private` constructor to block instantiation |
| 1:50–2:00 | Looking ahead | Inheritance starts next week — `extends`, `protected`, `super(...)` |

### Materials/Equipment
- Slides: "Static vs. Instance: One Copy vs. Many"
- Live-coding environment

### Formative Check (in-class)
Add a `static` instance counter to a class, increment it in every constructor, and write a
`static` getter for it; explain why the getter cannot read an instance field directly.

### Link to Lab/Assessment
Lab 4: Static vs. instance members (see `lab-manuals/lab-04.md`).
