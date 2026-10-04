# Week 7 — Lecture Content: 2D Arrays and Strings

## 1. Declaring and Indexing 2D Arrays
```java
int[][] grid = new int[2][3];    // 2 rows, 3 columns, all initialized to 0
int[][] literal = {
    {1, 2, 3},
    {4, 5, 6}
};
System.out.println(literal[1][0]);   // 4 — row 1, column 0
```
Java implements a 2D array as an **array of arrays**: `literal` is an array of 2 elements, each
itself an `int[]` of length 3. This means rows do not have to be the same length (a "jagged"
array), though for a regular grid they usually are.

## 2. Nested Loops Over a 2D Array
```java
int[][] grid = {
    {1, 2, 3},
    {4, 5, 6}
};

for (int row = 0; row < grid.length; row++) {
    for (int col = 0; col < grid[row].length; col++) {
        System.out.print(grid[row][col] + " ");
    }
    System.out.println();
}
```
`grid.length` is the number of rows; `grid[row].length` is the number of columns in that
particular row — always ask each row for its own length rather than assuming a fixed column
count, especially for jagged arrays.

## 3. Matrix Processing: Row and Column Sums
```java
static int rowSum(int[][] grid, int row) {
    int total = 0;
    for (int value : grid[row]) {
        total += value;
    }
    return total;
}

static int columnSum(int[][] grid, int col) {
    int total = 0;
    for (int[] row : grid) {
        total += row[col];
    }
    return total;
}
```

## 4. `String` Is an Immutable Object
```java
String greeting = "Hello";
String shouted = greeting.toUpperCase();   // creates a NEW String; greeting itself is unchanged
System.out.println(greeting);   // "Hello"
System.out.println(shouted);    // "HELLO"
```
Every `String` method that appears to "modify" a string actually returns a **new** `String`
object — the original is never changed. This is Java's **immutability** guarantee for `String`:
once created, a `String`'s character content can never change.

## 5. Common `String` Methods
```java
String s = "Programming Fundamentals";

int len = s.length();                  // 25
char c = s.charAt(0);                  // 'P'
String sub = s.substring(0, 11);       // "Programming"
int idx = s.indexOf("Fund");           // 12
boolean eq = s.equals("Programming Fundamentals");       // true — content equality
boolean eqIgnore = s.equalsIgnoreCase("PROGRAMMING FUNDAMENTALS"); // true
int cmp = "apple".compareTo("banana"); // negative — "apple" sorts before "banana"
```

## 6. `==` vs. `.equals()` for `String`
```java
String a = new String("test");
String b = new String("test");
String c = "test";
String d = "test";

System.out.println(a == b);        // false — two different String objects
System.out.println(a.equals(b));   // true  — same character content
System.out.println(c == d);        // true  — both refer to the same interned literal (an implementation detail!)
System.out.println(c.equals(d));   // true
```
`==` on reference types compares **whether two references point to the same object**, not
whether their contents are equal. `String` literals are sometimes shared (`c == d` happens to be
`true` here due to an internal optimization called string interning), but relying on that is a
bug waiting to happen — `a == b` is `false` even though the content is identical. **Always use
`.equals()` to compare `String` content, never `==`.**

## 7. `StringBuilder` for Efficient Concatenation
```java
StringBuilder sb = new StringBuilder();
for (int i = 1; i <= 5; i++) {
    sb.append(i).append(", ");
}
String result = sb.toString();
System.out.println(result);   // "1, 2, 3, 4, 5, "
```
Because `String` is immutable, writing `result = result + i` in a loop creates a brand-new
`String` object on *every* iteration, copying all the previous characters each time — wasteful
for many iterations. `StringBuilder` is a mutable companion class: `append` modifies the same
underlying buffer in place, making it far more efficient for building up text incrementally.

## 8. In-Class Exercise
Write a method that takes a 2D `int[][]` grid and returns the sum of each row as an `int[]`.
Then write a short program that builds a comma-separated string of the numbers 1–10 using
`StringBuilder`, and compare it (in a comment) to what repeated `+=` concatenation would cost.
