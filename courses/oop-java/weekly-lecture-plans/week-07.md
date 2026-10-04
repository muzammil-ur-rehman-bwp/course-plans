# Week 7 Lecture Plan — Object Oriented Programming (Java)
## Topic: Polymorphism I — Dynamic Dispatch, Upcasting/Downcasting, `instanceof`

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall that Java instance methods are virtual by default unless `final`/`private`/`static`. (*Remember*)
2. Explain upcasting, downcasting, and why Java references never slice an object. (*Understand*)
3. Apply `instanceof` to guard a safe downcast. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 6's `@Override`; quiz debrief |
| 0:10–0:35 | Dynamic dispatch | Calling an overridden method through a superclass-typed reference |
| 0:35–1:00 | No slicing in Java | Contrast with C++ by-value slicing; Java always holds references, never truncated copies |
| 1:00–1:25 | Upcasting/downcasting | Safe implicit upcast vs. explicit, checked downcast |
| 1:25–1:50 | `instanceof` | Guarding a downcast; brief look at the Java 16+ pattern-matching form |
| 1:50–2:00 | Looking ahead | Abstract classes next week; midterm review begins |

### Materials/Equipment
- Slides: "Polymorphism I: Dynamic Dispatch"
- Live-coding environment

### Formative Check (in-class)
Given an array of superclass-typed references actually holding mixed subclass objects, write a
loop that safely downcasts and calls a subclass-only method using `instanceof`.

### Link to Lab/Assessment
Lab 7: Polymorphism I (see `lab-manuals/lab-07.md`).
