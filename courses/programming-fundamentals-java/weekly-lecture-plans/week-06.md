# Week 6 Lecture Plan — Programming Fundamentals (Java)
## Topic: Arrays (1D)

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall array declaration, creation with `new`, and indexing syntax. (*Remember*)
2. Explain that arrays are objects on the heap with default element values. (*Understand*)
3. Apply loops to iterate over, fill, and aggregate values from an array. (*Apply*)
4. Analyze index-related bugs, including `ArrayIndexOutOfBoundsException`. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Declaring & creating arrays | Live-coded demo: `int[] a = new int[5];`, default values |
| 0:25–0:55 | Indexing & iteration | `for` and `for-each` loops over an array |
| 0:55–1:30 | Aggregates | Sum, max, min, average over an array (live-coded) |
| 1:30–1:50 | Passing arrays to methods | Mutating elements inside a method; caller sees the change |
| 1:50–2:00 | `ArrayIndexOutOfBoundsException` | Live demo of the exception and how to avoid it |

### Materials/Equipment
- Slides: "1D Arrays in Java"
- Live-coding environment

### Formative Check (in-class)
Quick exercise: write a method that takes an `int[]` and returns its maximum value; trace what
happens when called on an empty array.

### Link to Lab/Assessment
Lab 6: 1D arrays practice (see `lab-manuals/lab-06.md`). **Quiz 3 this week** (see
`quizzes/quiz-03.md`).
