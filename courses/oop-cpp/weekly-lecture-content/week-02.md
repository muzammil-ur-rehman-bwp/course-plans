# Week 2 — Lecture Content: Constructors in Depth; Destructors

## 1. Default and Parameterized Constructors
```cpp
class Point {
public:
    Point() : x_(0), y_(0) {}                    // default constructor
    Point(double x, double y) : x_(x), y_(y) {}  // parameterized constructor

    double getX() const { return x_; }
    double getY() const { return y_; }

private:
    double x_, y_;
};

Point origin;          // calls Point()
Point p(3.0, 4.0);     // calls Point(double, double)
```
A class can have several constructors (constructor overloading), each a different way to bring an
object into existence. If you define *any* constructor, the compiler stops generating a default
one for you — if you still need `Point()`, you must write it yourself, as above.

## 2. Member Initializer Lists
```cpp
class Circle {
public:
    Circle(double radius) : radius_(radius), area_(3.14159 * radius * radius) {}
    // NOT this:
    // Circle(double radius) {
    //     radius_ = radius;              // this is ASSIGNMENT, after the member already exists
    //     area_ = 3.14159 * radius * radius;
    // }

private:
    double radius_;
    double area_;
};
```
Members in the initializer list (`: radius_(radius), area_(...)`) are *constructed* with those
values directly. Code in the constructor *body* runs after every member already exists — so
assigning there is a two-step "default-construct, then overwrite," not direct initialization. For
most types the difference is only efficiency, but for two kinds of members it is not optional:
```cpp
class Wrapper {
public:
    Wrapper(int& target) : ref_(target), id_(42) {}  // MUST use initializer list

private:
    int& ref_;        // a reference: must be bound at construction, cannot be assigned later
    const int id_;    // a const member: must be initialized once, cannot be assigned later
};
```
Reference members and `const` members have no default state to assign into later — they *must*
be given their value in the initializer list, or the code does not compile. Members are
constructed in the order they are **declared in the class**, not the order they are listed in the
initializer list (good compilers warn if the two disagree).

## 3. Copy Constructors
```cpp
class Point {
public:
    Point(double x, double y) : x_(x), y_(y) {}
    Point(const Point& other) : x_(other.x_), y_(other.y_) {}   // copy constructor

private:
    double x_, y_;
};

Point a(1.0, 2.0);
Point b(a);          // copy constructor: b is a new, independent copy of a
Point c = a;         // also the copy constructor (initialization, not assignment)
void show(Point p);  // pass-by-value: copy constructor runs for the parameter
```
A **copy constructor** has the signature `ClassName(const ClassName& other)`. If you don't write
one, the compiler generates one that copies each member — which is correct for simple value types
like `Point`, but dangerously wrong for a class that owns a resource (next section). The copy
constructor runs whenever an object is passed by value, returned by value (in older compilers
without move optimizations), or explicitly copy-initialized as with `b` and `c` above.

## 4. Destructors
```cpp
class FileHandle {
public:
    FileHandle(const std::string& name) : file_(std::fopen(name.c_str(), "r")) {}

    ~FileHandle() {              // destructor: no return type, no parameters, name = ~ClassName
        if (file_ != nullptr) {
            std::fclose(file_);  // release the resource automatically
        }
    }

private:
    std::FILE* file_;
};
```
A **destructor** (`~ClassName()`) runs automatically when an object's lifetime ends — when a
local object goes out of scope, or when `delete` is applied to a pointer to a heap-allocated
object. It is the natural place to release any resource the object owns (a file handle, dynamic
memory, a network connection). A class has exactly one destructor; it takes no arguments and
cannot be overloaded.

## 5. The Rule of Three (Introduced Here, Revisited All Semester)
If a class needs to write **any one** of: a destructor, a copy constructor, or a copy-assignment
operator (`operator=`, covered formally in Week 3), it almost always needs to write **all three**
— this is the **Rule of Three**. Why: needing a custom destructor usually means the class owns a
resource (a raw pointer, a handle); the compiler-generated copy constructor and `operator=` only
copy *members*, which for an owning raw pointer means two objects end up pointing at the *same*
resource — then one destructor frees it, and the other's destructor frees it *again* (a double
free), or an object is left holding a dangling pointer after the original owner's destructor ran.
```cpp
class LeakyBuffer {
public:
    LeakyBuffer(int size) : data_(new int[size]), size_(size) {}
    ~LeakyBuffer() { delete[] data_; }
    // NO copy constructor written -> compiler generates one that copies data_ (the pointer
    // value, not the array!) -> two LeakyBuffer objects now share one array -> double free.

private:
    int* data_;
    int size_;
};
```
This semester you will meet the full version (the **Rule of Five**, adding move constructor/move
assignment) only in passing; the practical takeaway for now is: **a raw owning pointer member is a
liability** — either write all three copy-control members correctly, or (preferred, Week 15) use
a smart pointer so the compiler-generated copy control is safe by construction.

## 6. In-Class Exercise
Write a class `Buffer` that owns a dynamically allocated `int[]` of a given size, with a correct
destructor *and* a correct (deep-copying) copy constructor. Verify with a small `main` that
copying a `Buffer` and modifying the copy does not affect the original.
