# Week 11 Lecture Plan — Object Oriented Programming (Java)
## Topic: Generics II — Generic Methods, Wildcards, Bridge to Collections

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall generic class syntax from Week 10. (*Remember*)
2. Explain a generic method's own type parameter and wildcard bounds. (*Understand*)
3. Apply a generic method to write reusable, type-independent code. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | Week 10's `Box<T>`/`Pair<T,U>` |
| 0:10–0:35 | Generic methods | Live-coded generic `printAll`/`max` with its own `<T>` |
| 0:35–1:00 | Wildcards | `<? extends T>`/`<? super T>` read from a standard-library signature (brief) |
| 1:00–1:30 | Bridge to Collections | Previewing `List<T>`/`Map<K,V>` as generic classes built from this week's ideas |
| 1:30–1:50 | Capstone proposal workshop | Work time: sketch a class hierarchy for the capstone proposal due this week |
| 1:50–2:00 | Looking ahead | Exception handling next week |

### Materials/Equipment
- Slides: "Generics II: Methods & Wildcards"
- Live-coding environment

### Formative Check (in-class)
Write a generic method `<T extends Comparable<T>> T max(List<T> items)` and trace which concrete
type is used for a call with a `List<Integer>`.

### Link to Lab/Assessment
Lab 11: Generic methods (see `lab-manuals/lab-11.md`).
**Capstone proposal due** this week (see `assignments/capstone-proposal-guidelines.md`).
**Assignment 3 assigned** this week (Weeks 8–10), due start of Week 13.
