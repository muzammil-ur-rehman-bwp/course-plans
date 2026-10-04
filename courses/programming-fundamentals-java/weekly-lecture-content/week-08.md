# Week 8 — Lecture Content: Intro to Objects & References; Midterm Review

## 1. How Java References Work
```java
int[] a = {1, 2, 3};
int[] b = a;        // b now refers to the SAME array object as a — no copy is made

b[0] = 99;
System.out.println(a[0]);   // 99 — because a and b refer to the same object
```
A variable of a reference type (any array, `String`, or object of a class) does not hold the
object itself — it holds a **reference** to an object that lives elsewhere (conceptually, on the
heap). Assigning `b = a` copies the *reference*, not the array, so `a` and `b` end up pointing at
one shared array. Mutating the array through either variable is visible through the other.

Contrast this with primitives:
```java
int x = 5;
int y = x;   // y gets its OWN copy of the value 5
y = 10;
System.out.println(x);   // 5 — unaffected, because x and y are independent values
```

## 2. The Stack and the Heap (Conceptual Model)
- **Stack**: holds local variables and parameters for each in-progress method call. A primitive
  local variable's value lives directly on the stack. A reference-type local variable's *reference*
  (not the object) lives on the stack.
- **Heap**: holds every object and array actually created with `new` (or an array/string
  literal). An object on the heap is only reachable through some reference to it; Java's garbage
  collector automatically reclaims an object's heap memory once nothing references it anymore —
  there is no manual `delete`/`free` in Java, unlike some other languages.

```
Stack                    Heap
------                    ----
a  ----\
         \--> [1, 99, 3]   (one array object)
b  ----/
```

## 3. `null`
```java
int[] data = null;        // data currently refers to no object at all
System.out.println(data.length);   // throws NullPointerException — nothing to ask for the length of
```
`null` is a special reference value meaning "refers to no object." Any attempt to call a method
or access a field/element through a `null` reference throws a `NullPointerException` (often
abbreviated "NPE") — one of the most common runtime errors in Java. The fix is always to check for
`null` (or ensure the reference was properly initialized) before using it:
```java
if (data != null) {
    System.out.println(data.length);
} else {
    System.out.println("No data yet.");
}
```

## 4. `==` vs. `.equals()` for Objects in General
```java
int[] x = {1, 2, 3};
int[] y = {1, 2, 3};

System.out.println(x == y);         // false — two different array objects, even though contents match
System.out.println(x.equals(y));    // false too, for arrays specifically! (Object's default .equals is ==)
```
The general rule from Week 7 extends beyond `String`: `==` on any reference type compares object
identity (same object in memory?), not content. Note that arrays are a partial exception in
practice — unless a class explicitly overrides `.equals()` to compare content (as `String` does),
the default `.equals()` inherited from `Object` behaves exactly like `==`. (Comparing array
*contents* properly uses `java.util.Arrays.equals(x, y)`, not shown on the exam but worth
knowing.)

## 5. Midterm Review: Topic Map (Weeks 1–7)
- **Week 1–2**: JVM compile-run model, primitive types, operators, casting.
- **Week 3–4**: `if`/`switch` branching, `for`/`while`/`do-while` loops.
- **Week 5**: methods, pass-by-value, overloading.
- **Week 6–7**: 1D/2D arrays, `String` immutability, `StringBuilder`.
- **This week**: references, `null`, stack/heap, `==` vs. `.equals()`.

## 6. In-Class Exercise
Given two `int[]` variables assigned the same array (`int[] b = a;`), predict on paper what
`a[0]` prints after `b[0] = 99;` — then verify by running the code. Review at least 3 practice
problems spanning methods, arrays, and strings in preparation for the midterm.
