# Week 2 Lecture Plan — Object Oriented Programming (Java)
## Topic: Constructors in Depth; `equals`/`hashCode`/`toString`; No Destructors

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall how garbage collection removes the need for a destructor. (*Remember*)
2. Explain constructor chaining with `this(...)` and the `equals`/`hashCode` contract. (*Understand*)
3. Apply overloaded constructors and a correct `equals`/`hashCode`/`toString` trio to a class. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 1's encapsulated class as this week's starting point |
| 0:10–0:35 | Overloaded constructors & `this(...)` | Writing two or three constructors that chain via `this(...)` |
| 0:35–1:00 | `toString()` | Overriding `toString()` for readable `System.out.println(obj)` output |
| 1:00–1:30 | `equals(Object)` & `hashCode()` | Live-coded demo: the `equals(MyClass o)` overload bug vs. a correct `equals(Object o)` override; why `hashCode()` must change whenever `equals()` does |
| 1:30–1:50 | No destructors | Garbage collection recap from CS1; why Java has no Rule of Three to track |
| 1:50–2:00 | Looking ahead | Teaser: composition next week — objects as fields of other classes |

### Materials/Equipment
- Slides: "Constructors, Object Methods & Garbage Collection"
- Live-coding environment

### Formative Check (in-class)
Given a class with a buggy `equals(MyClass other)` method, identify why it is an overload, not an
override, fix the signature, and add a matching `hashCode()`.

### Link to Lab/Assessment
Lab 2: Constructors and the `Object` contract (see `lab-manuals/lab-02.md`).
**Quiz 1** this week — Week 1 material (see `quizzes/quiz-01.md`).
