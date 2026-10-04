# Lab Manual 2 — Constructors & Destructors

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Practice writing default/parameterized/copy constructors with member initializer lists, and
destructors, while applying the Rule of Three.

## Setup
Create `lab02.cpp` in your working folder.

## Procedure
1. **Task A — Constructors:** write `class Point3D` with private `x_, y_, z_`; a default
   constructor (all zero) and a parameterized constructor, both using member initializer lists.
2. **Task B — Owning resource:** write `class IntBuffer` that owns a dynamically allocated
   `int[]` of a given size (`new int[size]` in the constructor), with a correct destructor that
   `delete[]`s it.
3. **Task C — Rule of Three:** add a correct, deep-copying copy constructor to `IntBuffer`.
   Demonstrate (with a comment or a disabled code block) what would go wrong if you relied on the
   compiler-generated copy constructor instead.
4. **Task D — Mini-challenge:** write a function `IntBuffer makeFilledBuffer(int size, int
   value)` that returns an `IntBuffer` by value, filled with `value` in every slot, and call it in
   `main`, printing the result to confirm the copy/return behaves correctly.

## Expected Output
A single source file with four clearly labeled sections (A–D), compiling cleanly with
`-Wall`, with no memory errors (verify with the sanitizer/valgrind tip in `lab-notes/lab-02.md`
if available in your environment).

## Submission
Submit `lab02.cpp` via the course submission system by the end of the lab session.
