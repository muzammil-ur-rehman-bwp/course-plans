# Week 2 — Lecture Content: Operators, Expressions, and Type Conversion

## 1. Arithmetic, Relational, and Logical Operators
```java
int a = 7, b = 2;

int sum = a + b;         // 9
int diff = a - b;        // 5
int product = a * b;     // 14
int quotient = a / b;    // 3  — integer division truncates!
int remainder = a % b;   // 1  — modulo

boolean isGreater = a > b;              // true
boolean isEqualOrGreater = a >= b;      // true
boolean bothPositive = (a > 0) && (b > 0); // logical AND
boolean eitherNegative = (a < 0) || (b < 0); // logical OR
boolean notEqual = (a != b);            // true
```
Java's operators mirror most C-family languages: `+ - * / %` (arithmetic), `< <= > >= == !=`
(relational), `&& || !` (logical), and `= += -= *= /= %=` (assignment and compound assignment).

## 2. Operator Precedence and Increment/Decrement
```java
int x = 5;
int result = 2 + 3 * x;   // multiplication before addition: 2 + 15 = 17

x++;   // post-increment: use x, then increment (x becomes 6)
++x;   // pre-increment: increment, then use x (x becomes 7)
```
When in doubt about precedence, add parentheses — they cost nothing and remove ambiguity for the
next reader (including your future self).

## 3. Integer Division and the Casting Trap
```java
int a = 7, b = 2;
System.out.println(a / b);              // 3 — integer division truncates toward zero
System.out.println((double) a / b);     // 3.5 — one operand cast to double first
System.out.println(a / (double) b);     // 3.5 — same effect, either operand works
```
`(double) a` is an explicit **cast**: it tells the compiler "treat this `int` as a `double` for
this expression." Casting *after* the division (`(double) (a / b)`) does **not** fix anything —
by then the integer division has already truncated the result to `3`, and casting `3` to
`3.0` does not recover the lost fraction. The cast must happen before the division.

## 4. Implicit Widening vs. Explicit Narrowing
```java
int i = 42;
double d = i;        // implicit widening: int -> double, always safe, no cast needed

double pi = 3.99;
int truncated = (int) pi;   // explicit narrowing cast required: truncated == 3, not rounded
```
Java allows **implicit widening** (a smaller type automatically converts to a larger/compatible
one, e.g. `int` to `double`) because no information can be lost. **Narrowing** (e.g. `double` to
`int`) always requires an explicit cast, because information (the fractional part) may be lost —
the compiler forces you to acknowledge that explicitly.

## 5. Overflow and Precision Pitfalls
```java
int max = Integer.MAX_VALUE;     // 2147483647
System.out.println(max + 1);     // -2147483648 — silently wraps around (overflow)!

double d1 = 0.1 + 0.2;
System.out.println(d1);          // 0.30000000000000004 — binary floating-point imprecision
```
`int` arithmetic does **not** throw an error on overflow — it silently wraps around. Floating-
point types (`double`, `float`) cannot represent every decimal value exactly, so small rounding
errors are normal and expected; never compare two `double`s with `==` when you mean "close
enough" — compare `Math.abs(a - b) < epsilon` instead.

## 6. Primitive Types vs. Reference Types (Preview)
```java
int x = 5;            // x directly holds the value 5
String s = "hello";   // s holds a reference to a String object elsewhere in memory
```
Java has exactly eight **primitive types** (`int`, `long`, `short`, `byte`, `double`, `float`,
`char`, `boolean`) — a variable of one of these holds its value directly. Every other type
(`String`, arrays, and any class you or the JDK define) is a **reference type** — a variable of
one of these holds a reference (an address, conceptually) to an object stored elsewhere. This
distinction drives how assignment, comparison, and method parameters behave, and is covered in
depth in Week 8.

## 7. In-Class Exercise
Given `int a = 7, b = 2;`, predict the output of `a / b` and `(double) a / b` on paper, then
write and run a program to confirm. Then write a short program demonstrating `int` overflow by
adding `1` to `Integer.MAX_VALUE`.
