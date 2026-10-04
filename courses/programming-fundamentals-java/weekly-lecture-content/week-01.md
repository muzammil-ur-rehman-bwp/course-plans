# Week 1 — Lecture Content: Intro to Programming & Java Basics

## 1. What Is a Program?
A Java program is plain text (source code) that goes through two stages before it runs:
1. **Compile** — `javac` translates a `.java` source file into **bytecode** (a `.class` file),
   catching syntax and type errors.
2. **Run** — the **Java Virtual Machine (JVM)**, started with `java`, loads the bytecode and
   executes it. The JVM is what makes Java "write once, run anywhere": the same `.class` file runs
   on any machine with a compatible JVM, regardless of underlying hardware or OS.

```bash
javac Hello.java   # produces Hello.class (bytecode)
java Hello          # the JVM runs Hello.class (no .class extension here)
```
Unlike a program compiled directly to native machine code, a Java program's bytecode is
interpreted/JIT-compiled by the JVM at run time. You don't need to know the JVM's internals to
write Java, but you do need to know the two-step compile-then-run cycle above.

## 2. Your First Program
```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, Java!");
    }
}
```
- Every Java program lives inside a **class**. The file must be named `Hello.java` — exactly
  matching the public class name, including case.
- `public static void main(String[] args)` is the program's entry point — execution always
  starts here. `static` means the method belongs to the class itself, not to any particular
  object (more on this in Week 13); `void` means it returns nothing; `String[] args` holds any
  command-line arguments.
- `System.out` is the standard output stream; `println` prints a line and a trailing newline
  (`print` omits the newline).

## 3. Variables and Primitive Types
```java
int age = 20;            // whole numbers
double price = 19.99;    // floating-point numbers
char grade = 'A';        // a single character
boolean passed = true;   // true/false
```
Java is **statically typed**: every variable has a fixed type, declared once, checked by the
compiler before the program ever runs.

| Type | Holds | Size |
|---|---|---|
| `int` | whole numbers | 4 bytes |
| `double` | floating-point numbers | 8 bytes |
| `char` | a single character (Unicode) | 2 bytes |
| `boolean` | `true`/`false` | JVM-dependent (conceptually 1 bit of information) |

These four, along with `long`, `short`, `byte`, and `float`, are Java's **primitive types** —
they hold their value directly, not a reference to an object. (Week 2 contrasts this with
*reference types* like `String` and arrays.)

A local variable must be initialized before it is read:
```java
int count;
// System.out.println(count);  // COMPILE ERROR: variable count might not have been initialized
int total = 0;                  // always prefer declaring with an initial value
```
Unlike some languages, Java will not let this compile at all if the uninitialized variable is
read — the compiler enforces "definite assignment" for local variables. This is a real safety
advantage over languages that silently read garbage memory.

## 4. Basic Input and Output
```java
import java.util.Scanner;

public class RectangleArea {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("Enter width and height: ");
        double width = input.nextDouble();
        double height = input.nextDouble();

        double area = width * height;
        System.out.println("Area = " + area);

        input.close();
    }
}
```
- `import java.util.Scanner;` makes the `Scanner` class available.
- `new Scanner(System.in)` creates a `Scanner` that reads from the keyboard.
- `nextDouble()` reads the next whitespace-separated token and parses it as a `double`; `nextInt()`
  and `next()` (a single word) are the `int` and `String` equivalents.
- Closing the `Scanner` when done is good practice, though for a `Scanner` wrapping `System.in` in
  a simple console program it is not strictly required until the program exits.

## 5. Why Java Enforces So Much at Compile Time
Java's compiler is deliberately strict compared to some languages: every variable must have a
declared type, every local variable must be initialized before use, and every statement must end
in a semicolon. This is a trade-off — more upfront ceremony in exchange for catching entire
categories of bugs (type mismatches, uninitialized reads) before the program ever runs, rather
than discovering them as a crash later.

## 6. In-Class Exercise
Write a program that declares an `int` and a `double`, reads both from the user with `Scanner`,
computes their sum, and prints it with a descriptive label (e.g., `"Sum = 12.5"`).
