# Week 10 — Lecture Content: Templates I — Function Templates

## 1. Motivation: Avoiding Near-Duplicate Overloads
```cpp
int maxOf(int a, int b) { return a > b ? a : b; }
double maxOf(double a, double b) { return a > b ? a : b; }
std::string maxOf(std::string a, std::string b) { return a > b ? a : b; }
```
These three functions are identical except for the type — exactly the kind of repetition
templates exist to eliminate. **Generic programming** lets you write the logic once, for a
placeholder type, and let the compiler generate the right version for each actual type used.

## 2. `template <typename T>` Function Syntax
```cpp
template <typename T>
T maxOf(T a, T b) {
    return (a > b) ? a : b;
}

int i = maxOf(3, 7);                 // T deduced as int
double d = maxOf(2.5, 1.1);          // T deduced as double
std::string s = maxOf(std::string("apple"), std::string("banana"));  // T deduced as std::string
```
`template <typename T>` declares `T` as a placeholder type parameter for the function that
follows. The compiler does not generate any code until the template is actually used — at each
call site, it deduces `T` from the arguments and generates (**instantiates**) a concrete version
of the function for that specific type.

## 3. Template Argument Deduction
```cpp
template <typename T>
void printTwice(T value) {
    std::cout << value << " " << value << "\n";
}

printTwice(42);        // T = int
printTwice(3.14);       // T = double
printTwice("hello");    // T = const char*  (not std::string! a string literal's type)
```
The compiler deduces `T` purely from the types of the arguments passed at the call site — there
is no need to write `printTwice<int>(42)` when deduction is unambiguous. Note the last call: a
string literal's type is `const char*`, not `std::string`, unless you pass an actual
`std::string` object — a common surprise worth tracing through carefully.

## 4. Explicit Instantiation
```cpp
template <typename T>
T maxOf(T a, T b) { return (a > b) ? a : b; }

// maxOf(3, 4.5);          // ERROR: deduction is ambiguous — is T int or double?
maxOf<double>(3, 4.5);      // explicit: force T = double; 3 converts to 3.0
```
When the arguments have *different* types, deduction cannot pick a single `T` on its own, and the
call fails to compile unless you **explicitly specify** the type parameter with `<double>` (as
above) — or change the function to take two independent type parameters, which we revisit for
class templates next week.

## 5. Implicit Constraints on `T`
```cpp
template <typename T>
T maxOf(T a, T b) { return (a > b) ? a : b; }

struct Point { int x, y; };
Point p1{1, 2}, p2{3, 4};
// maxOf(p1, p2);   // ERROR: no operator> defined for Point — fails inside the template body
```
A function template places no explicit restriction on `T` in its signature, but its **body**
implicitly requires whatever operations it uses (`>` here) to be supported by `T` — this is often
called an implicit constraint. If `T` doesn't support an operation the template body needs, the
compilation fails at the point the template is instantiated for that `T`, typically with an error
message pointing inside the template's body rather than at the call site — a common source of
confusing-looking compiler errors worth recognizing early.

## 6. In-Class Exercise
Write a function template `template <typename T> void swapValues(T& a, T& b)` that swaps two
values of any type using a temporary (no `std::swap`), and test it with `int`, `double`, and
`std::string` arguments. Then write a template `template <typename T> void printAll(const
std::vector<T>& values)` that prints every element of any vector, separated by spaces.
