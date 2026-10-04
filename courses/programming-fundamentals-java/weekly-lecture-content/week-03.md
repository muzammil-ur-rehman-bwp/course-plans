# Week 3 — Lecture Content: Control Flow — `if`/`else`, `switch`

## 1. Boolean Expressions
Any expression that evaluates to `true` or `false` can drive a conditional:
```java
int age = 20;
boolean canVote = age >= 18;
```

## 2. `if` / `else if` / `else`
```java
int score = 72;
char grade;

if (score >= 85) {
    grade = 'A';
} else if (score >= 70) {
    grade = 'B';
} else if (score >= 55) {
    grade = 'C';
} else if (score >= 40) {
    grade = 'D';
} else {
    grade = 'F';
}
System.out.println("Grade: " + grade);
```
Conditions are checked top to bottom; the first one that is `true` runs, and the rest are
skipped. Order matters — if the bands above were tested in the wrong order (e.g., `score >= 40`
first), every score of 40+ would incorrectly match the first, loosest band.

## 3. The Dangling-`else` Pitfall
```java
int x = 7;

if (x > 5)
    if (x > 10)
        System.out.println("big");
    else
        System.out.println("medium");   // this else binds to the INNER if, not the outer one
```
An `else` always binds to the nearest unmatched `if`. When nesting conditionals, use braces `{}`
even for single-statement bodies — it costs nothing and removes any ambiguity about which `if`
an `else` belongs to.

## 4. The `switch` Statement
```java
int day = 3;
String name;

switch (day) {
    case 1:
        name = "Monday";
        break;
    case 2:
        name = "Tuesday";
        break;
    case 3:
        name = "Wednesday";
        break;
    default:
        name = "Unknown";
        break;
}
System.out.println(name);
```
`switch` compares one value against several constant cases. **Forgetting `break` causes
fall-through** — execution continues into the next case's code rather than exiting the switch.
Fall-through is occasionally intentional (e.g., grouping several cases to share one action) but
is a common source of bugs when accidental.

## 5. The Modern Switch Expression (Java 14+)
```java
int day = 3;
String name = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    case 3 -> "Wednesday";
    default -> "Unknown";
};
System.out.println(name);
```
The arrow form introduced in Java 14 is an **expression** — it produces a value directly, has no
fall-through, and needs no `break`. It is generally preferred in new code; the traditional
`case: ... break;` form is still shown here because it remains common in existing code and in
many textbooks.

## 6. The Ternary (Conditional) Operator
```java
int a = 7, b = 2;
int max = (a > b) ? a : b;   // "if a > b then a, else b"
```
The ternary operator is a compact `if`/`else` for choosing between two *values* (not statements).
Use it for simple value selection; prefer a full `if`/`else` when the logic involves multiple
statements or is hard to read on one line.

## 7. In-Class Exercise
Write an `if`/`else if`/`else` chain assigning a letter grade from a numeric score (bands as in
Section 2), testing the boundary values (85, 70, 55, 40) explicitly. Then rewrite a small
menu-choice example (`1` = Add, `2` = Subtract, `3` = Multiply, else Unknown) using both a
traditional `switch` and an arrow-form `switch` expression.
