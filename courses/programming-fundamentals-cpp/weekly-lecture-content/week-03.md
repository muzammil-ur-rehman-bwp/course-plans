# Week 3 — Lecture Content: Control Flow — `if`/`else`, `switch`

## 1. The `if`/`else` Statement
```cpp
int score = 72;
if (score >= 85) {
    std::cout << "Grade: A\n";
} else if (score >= 70) {
    std::cout << "Grade: B\n";
} else if (score >= 55) {
    std::cout << "Grade: C\n";
} else {
    std::cout << "Grade: F\n";
}
```
Conditions are checked top to bottom; the first `true` branch runs and the rest are skipped. Order
matters — if the `>= 70` check were written first, a score of 90 would incorrectly report "B".

## 2. Braces and the Dangling-`else` Trap
```cpp
// Risky: no braces — easy to misread or break when adding a line later
if (score >= 70)
    std::cout << "Pass\n";
else
    std::cout << "Fail\n";

// Safer: always brace, even for single statements
if (score >= 70) {
    std::cout << "Pass\n";
} else {
    std::cout << "Fail\n";
}
```
Always brace `if`/`else` bodies, even one-liners. It costs nothing and prevents a very common bug:
adding a second statement to a brace-less branch, which silently falls outside the conditional.

## 3. The `switch` Statement
```cpp
int dayNumber = 3;
switch (dayNumber) {
    case 1:
        std::cout << "Monday\n";
        break;
    case 2:
        std::cout << "Tuesday\n";
        break;
    case 3:
        std::cout << "Wednesday\n";
        break;
    default:
        std::cout << "Unknown day\n";
        break;
}
```
`switch` compares an integer (or `char`/`enum`) value against a set of constant `case` labels.
**Omitting `break` causes fall-through** — execution continues into the next `case` — which is
occasionally used deliberately (e.g., grouping several cases) but is a frequent source of bugs
when accidental:
```cpp
switch (dayNumber) {
    case 6:
    case 7:
        std::cout << "Weekend\n";  // cases 6 and 7 share this body (intentional fall-through)
        break;
    default:
        std::cout << "Weekday\n";
        break;
}
```
`switch` only tests equality against constants — it cannot express ranges (`score >= 70`) the way
`if`/`else` can.

## 4. The Ternary (Conditional) Operator
```cpp
int a = 5, b = 10;
int max = (a > b) ? a : b;   // reads: "if a > b then a, else b"
```
Useful for short, simple either/or expressions; for anything with multiple conditions or
side effects, prefer a full `if`/`else` for readability.

## 5. In-Class Exercise
Write a program that reads an integer month number (1–12) and prints the number of days in that
month (assume 28 for February), using a `switch` statement with grouped fall-through cases for
months sharing the same day count.
