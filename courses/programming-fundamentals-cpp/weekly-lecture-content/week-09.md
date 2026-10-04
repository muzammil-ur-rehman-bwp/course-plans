# Week 9 — Lecture Content: Midterm + Dynamic Memory

## 1. Midterm Exam
Covers Weeks 1–8 (C++ basics, operators, control flow, loops, functions, arrays, strings,
pointers/references). See the Week 8 review materials for practice problems.

## 2. Stack vs. Heap
- **Stack memory**: automatic — local variables are allocated when their scope is entered and
  freed automatically when it is exited. Fast, but size is fixed at compile time and lifetime is
  tied to scope.
- **Heap memory**: manual — memory requested at runtime with `new`, which exists until explicitly
  released with `delete`, independent of any function's scope. This lets data outlive the
  function that created it, or have a size determined only at runtime.

## 3. Allocating a Single Object
```cpp
int* p = new int;        // allocates one uninitialized int on the heap
*p = 42;
std::cout << *p << "\n"; // 42
delete p;                 // required — frees the heap memory
p = nullptr;               // good practice: avoid leaving a dangling pointer around
```
Every `new` must be matched by exactly one `delete`. Forgetting `delete` causes a **memory leak**
— the memory stays reserved for the life of the program, even though nothing can reach it anymore.

## 4. Allocating a Dynamic Array
```cpp
int n;
std::cout << "How many scores? ";
std::cin >> n;

int* scores = new int[n];   // size decided at runtime, unlike a fixed-size array
for (int i = 0; i < n; ++i) {
    scores[i] = i * 10;
}
std::cout << scores[2] << "\n";

delete[] scores;             // array form of new MUST be freed with delete[], not delete
scores = nullptr;
```
Mismatching `new`/`delete` with `new[]`/`delete[]` is undefined behavior — always match the form.

## 5. Dangling Pointers
```cpp
int* p = new int(5);
delete p;
// p now dangles — it still holds the old address, but that memory is no longer valid
std::cout << *p << "\n";   // undefined behavior: reading through a dangling pointer
```
A **dangling pointer** points to memory that has already been freed. Using it (reading, writing,
or deleting it again) is undefined behavior. Setting a pointer to `nullptr` immediately after
`delete` does not fix the underlying design issue but does make an accidental later use fail
loudly (dereferencing `nullptr` crashes) rather than silently corrupting memory.

## 6. Why This Matters
Unlike languages with automatic garbage collection, C++ requires the programmer to track every
heap allocation's lifetime by hand. The discipline introduced here — one `delete`/`delete[]` for
every `new`/`new[]`, never using a pointer after it's freed — is foundational for everything built
on dynamic memory later (and for understanding why modern C++ increasingly favors container types
like `std::vector`, which manage this automatically, over raw `new`/`delete`).

## 7. In-Class Exercise
Write a program that asks the user for a size `n`, dynamically allocates an array of `n` doubles,
fills it with user input, computes the average, prints it, and correctly frees the memory.
