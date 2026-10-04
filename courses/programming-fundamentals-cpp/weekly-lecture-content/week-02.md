# Week 2 — Lecture Content: Operators, Expressions, and Type Conversion

## 1. Arithmetic Operators
```cpp
int a = 7, b = 2;
std::cout << a + b << "\n";  // 9
std::cout << a - b << "\n";  // 5
std::cout << a * b << "\n";  // 14
std::cout << a / b << "\n";  // 3  (integer division truncates toward zero)
std::cout << a % b << "\n";  // 1  (remainder; only defined for integer operands)
```
**Integer division is the single most common source of surprise for beginners.** `7 / 2` is `3`,
not `3.5`, because both operands are `int`. To get a fractional result, at least one operand must
be a floating-point type:
```cpp
double result = a / static_cast<double>(b);  // 3.5
```

## 2. Compound Assignment and Increment/Decrement
```cpp
int count = 0;
count += 5;   // count = count + 5
count *= 2;   // count = count * 2
count++;      // post-increment: use count, then add 1
++count;      // pre-increment: add 1, then use count
```
In a standalone statement `count++;` and `++count;` behave the same; they differ only when the
expression's *value* is also used (e.g., inside a larger expression), which this course avoids for
clarity — write increments as their own statement.

## 3. Relational and Logical Operators
```cpp
int x = 5, y = 10;
bool isEqual = (x == y);        // false
bool isLess  = (x < y);         // true
bool combo   = (x < y) && (y < 20);  // true (logical AND)
bool either  = (x > y) || (x < 0);   // false (logical OR)
bool negate  = !isEqual;        // true (logical NOT)
```
A classic beginner bug: writing `x = y` (assignment) inside a condition where `x == y`
(comparison) was intended. Modern compilers warn about this with `-Wall`; always read the warning.

## 4. Operator Precedence
Operators evaluate in a defined order (arithmetic before relational before logical, `*`/`/` before
`+`/`-`), but **relying on memorized precedence tables makes code hard to read.** Use parentheses
to make intent explicit:
```cpp
// Technically correct without parens, but less readable:
bool valid = x > 0 && x < 100 || y == 0;

// Clearer:
bool valid = (x > 0 && x < 100) || (y == 0);
```

## 5. Type Conversion
- **Implicit conversion**: the compiler converts automatically when types mix, following
  well-defined promotion rules (e.g., `int` → `double` in a mixed expression).
- **Explicit conversion (casting)**: the programmer requests a conversion with `static_cast<T>`,
  making intent clear and avoiding accidental implicit conversions.
```cpp
int wholeApples = 5;
int totalPeople = 2;
double average = static_cast<double>(wholeApples) / totalPeople;  // 2.5, not 2
```
Avoid the old C-style cast `(double)wholeApples` in new C++ code — `static_cast` is checked more
strictly by the compiler and communicates intent more clearly to readers.

## 6. In-Class Exercise
Given `int a = 9, b = 4;`, predict and then verify by compiling: `a / b`, `a % b`,
`static_cast<double>(a) / b`, and `(a + b) * 2 > 20`.
