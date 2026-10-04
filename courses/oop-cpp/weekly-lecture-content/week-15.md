# Week 15 — Lecture Content: RAII & Smart Pointers; Debugging/Testing OOP Code

## 1. RAII, Revisited
Since Week 2, every destructor you've written has followed **Resource Acquisition Is
Initialization (RAII)**: a resource (memory, a file, a lock) is acquired in a constructor and
released in the matching destructor, which runs automatically — on normal scope exit, *and*
during exception-driven stack unwinding (Week 12). Smart pointers are simply RAII applied
specifically to the one resource C++ programmers manage most often: dynamically allocated memory.

## 2. The Problem With Raw Owning Pointers (Recap)
```cpp
class Buffer {
public:
    Buffer(int size) : data_(new int[size]) {}
    ~Buffer() { delete[] data_; }
    // No copy constructor written -> Week 2's Rule-of-Three bug: shallow copy, double free.
    // If a constructor after `new` throws, or an exception passes through, data_ leaks.
private:
    int* data_;
};
```
A raw owning pointer member requires you to get the destructor right (Week 2), the copy
constructor right (Rule of Three), *and* every exception path right (Week 12) — three
independent ways to get it wrong, in every class that owns memory this way.

## 3. `std::unique_ptr<T>`: Exclusive Ownership
```cpp
#include <memory>

class Buffer {
public:
    Buffer(int size) : data_(std::make_unique<int[]>(size)), size_(size) {}
    // NO destructor needed — unique_ptr's own destructor frees data_ automatically.
    // NO copy constructor needed — unique_ptr is move-only, so Buffer becomes move-only too,
    // which is CORRECT: it can no longer be silently, dangerously shallow-copied.

    int& at(int i) { return data_[i]; }

private:
    std::unique_ptr<int[]> data_;
    int size_;
};

std::unique_ptr<int> p = std::make_unique<int>(42);
std::cout << *p << "\n";                 // 42 — dereference like a raw pointer
// std::unique_ptr<int> q = p;            // ERROR: unique_ptr cannot be copied
std::unique_ptr<int> q = std::move(p);     // OK: ownership transfers to q; p is now empty
```
`std::unique_ptr<T>` owns exactly one object and frees it automatically when the `unique_ptr`
itself is destroyed — no manual `delete`, ever. It cannot be copied (only *moved*, transferring
ownership and leaving the source empty), which is exactly the right behavior for exclusive
ownership: there is never a moment where two `unique_ptr`s think they own the same object, so the
double-free bug from Week 2 cannot occur. Prefer `make_unique` over a bare `new` — it is exception
-safer and keeps `new`/`delete` out of your code entirely.

## 4. `std::shared_ptr<T>`: Shared Ownership
```cpp
std::shared_ptr<int> a = std::make_shared<int>(100);
std::shared_ptr<int> b = a;            // OK: shared_ptr CAN be copied — both now own the same int
std::cout << *a << " " << *b << "\n";   // 100 100
std::cout << a.use_count() << "\n";     // 2 — two shared_ptrs currently own this object
// The underlying int is freed automatically only once the LAST shared_ptr owning it is destroyed.
```
`std::shared_ptr<T>` allows multiple owners of the same object, tracked with a reference count:
each copy increments it, each destruction decrements it, and the object is freed exactly when the
count reaches zero. This is the right tool when an object's ownership is genuinely shared — e.g.
several parts of a program all need the same object to outlive whichever one of them finishes
first — but it costs more (reference-counting overhead) than `unique_ptr`, so **default to
`unique_ptr`** and reach for `shared_ptr` only when shared ownership is the actual requirement,
not simply because copying is convenient.

## 5. Choosing Between Them
```cpp
// Exclusive ownership: one owner, clear lifetime -> unique_ptr
class Engine { /* ... */ };
class Car {
    std::unique_ptr<Engine> engine_;   // only this Car owns its Engine
};

// Shared ownership: multiple owners, unclear single "owner" -> shared_ptr
class Scene {
    std::vector<std::shared_ptr<Texture>> sharedTextures_;   // many objects may reference the same texture
};
```
The design question is always "does exactly one thing own this, or can ownership be genuinely
shared?" — answer that first, then pick `unique_ptr` or `shared_ptr` accordingly. A raw pointer
is still appropriate for *non-owning* access (observing an object you don't own, e.g. a function
parameter that just looks at an object without managing its lifetime) — smart pointers replace
owning raw pointers, not every pointer use.

## 6. Debugging OOP Code
A debugger (`gdb`, or your IDE's integrated debugger) lets you set a breakpoint inside a
constructor or destructor and step through member initialization order (Weeks 2, 5, 6) exactly as
discussed in lecture — invaluable for confirming *when* and *in what order* constructors/
destructors actually run, rather than reasoning about it only on paper. Watch a `unique_ptr`/
`shared_ptr` member in the debugger when tracking down a suspected lifetime bug; a `use_count()`
that isn't what you expect usually points directly at the bug's location.

## 7. Basic Testing of OOP Code
```cpp
void testFractionAddition() {
    Fraction a(1, 2), b(1, 3);
    Fraction sum = a + b;
    assert(sum == Fraction(5, 6));   // <cassert>: aborts with a message if the condition is false
    std::cout << "testFractionAddition passed.\n";
}
```
Writing small, explicit test functions that call a class's public interface (constructors,
overloaded operators, virtual methods through a base pointer) and check results with `assert` or
manual comparison is a lightweight habit worth building now, ahead of any formal testing
framework — exercise constructors with both valid and invalid input, exercise overloaded
operators against hand-computed expected results, and exercise polymorphic behavior through base-
class pointers specifically (to catch slicing/missing-`override` bugs, Weeks 7–8).

## 8. In-Class Exercise
Take the `Buffer` class from section 2 (raw owning pointer) and rewrite it to use
`std::unique_ptr<int[]>` as in section 3, deleting the now-unnecessary destructor and copy
constructor. Then write three short test functions exercising a class from an earlier week
(e.g. `Stack<T>` from Week 11): one for normal use, one for an edge case (popping an empty stack),
and one using `assert` to check an expected result.
