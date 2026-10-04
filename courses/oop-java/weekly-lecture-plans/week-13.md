# Week 13 Lecture Plan — Object Oriented Programming (Java)
## Topic: The Collections Framework

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall `List<T>`/`Map<K,V>` as generic classes from Weeks 10–11. (*Remember*)
2. Explain why a `HashMap` key type needs a correct `equals`/`hashCode` pair. (*Understand*)
3. Apply `ArrayList`, `HashMap`, enhanced `for`, and a lambda-based `Comparator` to store, iterate, and sort objects. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 12's exception handling |
| 0:10–0:35 | `List`/`ArrayList` | Growable, type-safe arrays; common methods |
| 0:35–1:00 | `Map`/`HashMap` | Key-value storage; revisiting Week 2's `equals`/`hashCode` contract as a hard requirement for keys |
| 1:00–1:20 | Enhanced `for` | Iterating a `List`/`Map.entrySet()` |
| 1:20–1:50 | Lambdas & `Comparator` | Live-coded: sorting a `List<CustomObject>` with `list.sort((a, b) -> ...)` |
| 1:50–2:00 | Looking ahead | Software design (UML, SOLID) next week |

### Materials/Equipment
- Slides: "The Collections Framework"
- Live-coding environment

### Formative Check (in-class)
Given a `List<Student>`, sort it by GPA descending using a lambda-based `Comparator`, then by
name ascending as a tie-break using `Comparator.comparing(...).thenComparing(...)`.

### Link to Lab/Assessment
Lab 13: Collections Framework (see `lab-manuals/lab-13.md`).
**Assignment 4 assigned** this week (Weeks 11–13), due start of Week 15.
