# Week 1 — Lecture Content: Intro to Programming & C++ Basics

## 1. What Is a Program?
A C++ program is plain text (source code) that must be **compiled** into machine code before it
can run. The pipeline has three stages:
1. **Compile** — the compiler (`g++`/`clang++`) translates `.cpp` source into object code, catching
   syntax and type errors.
2. **Link** — the linker combines object code with library code (e.g., the standard library) into
   a single executable.
3. **Run** — the operating system loads and executes the resulting binary.

```bash
g++ -std=c++17 -Wall hello.cpp -o hello
./hello
```
`-std=c++17` selects the C++17 language standard; `-Wall` enables helpful warnings. Always compile
with warnings on — a program that compiles clean but with warnings suppressed is hiding bugs.

## 2. Your First Program
```cpp
#include <iostream>

int main() {
    std::cout << "Hello, C++!" << std::endl;
    return 0;
}
```
- `#include <iostream>` pulls in declarations for console input/output streams.
- `int main()` is the program's entry point — execution always starts here.
- `std::cout` ("console out") is the standard output stream; `<<` is the "insert" operator.
- `return 0;` signals successful completion to the operating system.

## 3. Variables and Primitive Types
```cpp
int age = 20;          // whole numbers
double price = 19.99;  // floating-point numbers
char grade = 'A';       // a single character
bool passed = true;    // true/false
```
C++ is **statically typed**: every variable has a fixed type, declared once, checked by the
compiler before the program ever runs. This is different from dynamically typed languages where a
variable's type is determined only at runtime.

| Type | Holds | Typical size |
|---|---|---|
| `int` | whole numbers | 4 bytes |
| `double` | floating-point numbers | 8 bytes |
| `char` | a single character | 1 byte |
| `bool` | `true`/`false` | 1 byte |

A variable must be declared (and normally initialized) before use:
```cpp
int count;        // declared, but uninitialized — reading it now is undefined behavior
int total = 0;     // declared and initialized — always prefer this form
```

## 4. Basic Input and Output
```cpp
#include <iostream>

int main() {
    int width, height;
    std::cout << "Enter width and height: ";
    std::cin >> width >> height;

    int area = width * height;
    std::cout << "Area = " << area << std::endl;
    return 0;
}
```
`std::cin >>` ("console in") reads whitespace-separated input into a variable, converting the
typed text into the variable's type. Chaining `std::cin >> width >> height` reads two values in
one statement.

## 5. Uninitialized Variables and Undefined Behavior
Unlike some languages, C++ does **not** automatically initialize local variables of primitive
type. Reading an uninitialized variable is undefined behavior — the program might print garbage,
crash, or (worst of all) appear to work by luck. The habit to build from day one: **always
initialize variables when you declare them.**

## 6. In-Class Exercise
Write a program that declares an `int` and a `double`, reads both from the user with `std::cin`,
computes their sum, and prints it with a descriptive label (e.g., `"Sum = 12.5"`).
