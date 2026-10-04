# Week 5 — Lecture Content: Functions

## 1. Why Functions?
Functions let us name a piece of logic once and reuse it, break a large problem into small
testable pieces, and give each piece a clear contract (inputs in, a result out). A `main()` that
is a single 200-line block is a warning sign; a `main()` that reads as a short sequence of
well-named function calls is the goal.

## 2. Declaration vs. Definition
```cpp
// Declaration (prototype) — tells the compiler the function's signature
double areaOfRectangle(double width, double height);

int main() {
    std::cout << areaOfRectangle(3.0, 4.0) << "\n";  // can call before seeing the definition
    return 0;
}

// Definition — provides the actual body
double areaOfRectangle(double width, double height) {
    return width * height;
}
```
A declaration lets other code call the function before the compiler has seen its full definition
— essential once programs span multiple functions or files.

## 3. Parameters: Pass-by-Value
```cpp
void tryToDouble(int n) {
    n = n * 2;  // modifies only the local copy
}

int main() {
    int x = 5;
    tryToDouble(x);
    std::cout << x << "\n";  // still 5 — the caller's variable is untouched
}
```
By default, C++ passes arguments **by value**: the function receives a copy. Changes inside the
function never affect the caller's variable.

## 4. Parameters: Pass-by-Reference
```cpp
void doubleIt(int& n) {
    n = n * 2;  // modifies the caller's variable directly
}

void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

int main() {
    int x = 5;
    doubleIt(x);
    std::cout << x << "\n";  // 10 — x itself changed

    int p = 1, q = 2;
    swap(p, q);
    std::cout << p << " " << q << "\n";  // 2 1
}
```
An `&` after the parameter type makes it a **reference parameter** — an alias for the caller's
variable, not a copy. Use pass-by-reference when a function must modify the caller's variable, or
to avoid copying a large object (combined with `const` where the function should only read it,
e.g., `const std::string& name`).

## 5. Default Arguments and Overloading
```cpp
double power(double base, int exponent = 2) {   // default argument
    double result = 1.0;
    for (int i = 0; i < exponent; ++i) result *= base;
    return result;
}

int area(int side) { return side * side; }                 // overload 1: square
int area(int width, int height) { return width * height; } // overload 2: rectangle
```
**Overloading** lets several functions share a name as long as their parameter lists differ (in
number or type); the compiler picks the matching version at each call site based on the arguments
given.

## 6. Scope and Lifetime
A variable declared inside a function (or inside a block, like a loop body) is **local**: it comes
into existence when its declaration runs and is destroyed when its enclosing block ends. A local
variable in one function is completely invisible to, and independent of, a same-named local
variable in another function.

## 7. In-Class Exercise
Write an overloaded pair of functions `int maxValue(int a, int b)` and
`int maxValue(int a, int b, int c)`, then a `void increment(int& n)` function, and verify in
`main` that `increment` changes the caller's variable while a pass-by-value version would not.
