# Week 6 — Lecture Content: Arrays (1D)

## 1. Declaring and Creating Arrays
```java
int[] scores;                 // declaration — scores is currently null
scores = new int[5];          // creation — allocates an array of 5 ints on the heap

int[] values = new int[5];    // declare and create in one statement
int[] literal = {10, 20, 30, 40, 50};  // declare, create, and initialize with literal values
```
An array in Java is itself an **object** (a reference type), even though it holds primitives.
`new int[5]` allocates space for 5 `int`s on the heap and returns a reference to it, stored in
`scores`. Numeric array elements default to `0` (`false` for `boolean`, `null` for arrays of
object references) if not explicitly initialized.

## 2. Indexing
```java
int[] values = {10, 20, 30, 40, 50};
System.out.println(values[0]);   // 10 — first element
System.out.println(values[4]);   // 50 — last element (index = length - 1)
System.out.println(values.length); // 5 — note: no parentheses, it's a field, not a method
```
Valid indices run from `0` to `values.length - 1`. Accessing `values[5]` or `values[-1]` compiles
fine but throws an `ArrayIndexOutOfBoundsException` at run time — Java always checks array bounds,
rather than silently reading adjacent memory.

## 3. Iterating Over an Array
```java
int[] values = {10, 20, 30, 40, 50};

// Classic indexed loop — use when you need the index itself
for (int i = 0; i < values.length; i++) {
    System.out.println("Index " + i + ": " + values[i]);
}

// for-each — cleaner when you only need each value, not its index
for (int v : values) {
    System.out.println(v);
}
```
`for-each` cannot modify the underlying array (it gives you a copy of each value, not a way to
write back into `values`), so use the classic indexed form whenever you need to assign into
`values[i]`.

## 4. Aggregates: Sum, Max, Min, Average
```java
static double average(int[] values) {
    if (values.length == 0) {
        return 0.0;   // guard against division by zero
    }
    int total = 0;
    for (int v : values) {
        total += v;
    }
    return (double) total / values.length;
}

static int max(int[] values) {
    int best = values[0];   // assumes at least one element — document or guard this precondition
    for (int i = 1; i < values.length; i++) {
        if (values[i] > best) {
            best = values[i];
        }
    }
    return best;
}
```

## 5. Passing Arrays to Methods
```java
static void doubleAll(int[] values) {
    for (int i = 0; i < values.length; i++) {
        values[i] = values[i] * 2;   // mutates the caller's array through the shared reference
    }
}

public static void main(String[] args) {
    int[] nums = {1, 2, 3};
    doubleAll(nums);
    System.out.println(nums[0]);   // 2 — the caller sees the change
}
```
Because arrays are reference types, a method does not need to return a new array just to modify
one in place — any change made to an element through the parameter is visible to the caller, as
covered in Week 5.

## 6. `ArrayIndexOutOfBoundsException`
```java
int[] values = new int[5];
System.out.println(values[5]);  // throws ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 5
```
This is a very common beginner bug, usually from an off-by-one error in a loop bound (`<=` where
`<` was meant, or starting a loop at `1` instead of `0`). Reading the exception message —
it names the bad index and the array's length — is usually enough to locate the bug immediately.

## 7. In-Class Exercise
Write a method `int max(int[] values)` that returns an array's maximum value. Discuss as a class:
what should it do if `values` has length `0`? Pick an approach (throw an exception, return a
sentinel, require the caller to check first) and justify it in a comment.
