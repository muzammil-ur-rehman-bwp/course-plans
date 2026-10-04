# Week 4 — Lecture Content: Loops — `for`, `while`, `do-while`

## 1. The `for` Loop
```java
for (int i = 1; i <= 5; i++) {
    System.out.println("Count: " + i);
}
```
A `for` loop packages initialization, condition, and update into one header — ideal when you know
in advance how many times to iterate (**counter-controlled** iteration).

## 2. The `while` Loop
```java
int total = 0;
int n = 0;

while (n < 5) {
    total += n;
    n++;
}
System.out.println("Total: " + total);
```
A `while` loop checks its condition before each iteration (including the first) — use it when the
number of iterations isn't known ahead of time.

## 3. The `do-while` Loop
```java
Scanner input = new Scanner(System.in);
int choice;

do {
    System.out.println("1. Add  2. Remove  3. Quit");
    choice = input.nextInt();
} while (choice != 3);
```
A `do-while` loop checks its condition **after** each iteration, guaranteeing the body runs at
least once — a natural fit for menu loops that must display the menu before checking whether the
user wants to quit.

## 4. Sentinel-Controlled Input
```java
Scanner input = new Scanner(System.in);
int total = 0;
int count = 0;
int value;

System.out.println("Enter numbers, -1 to stop:");
value = input.nextInt();
while (value != -1) {
    total += value;
    count++;
    value = input.nextInt();
}

if (count > 0) {
    System.out.println("Average: " + (double) total / count);
} else {
    System.out.println("No numbers entered.");
}
```
A **sentinel** is a special value (here, `-1`) that signals "stop" and is not itself treated as
real data. Note the guard against dividing by zero when `count` is `0`.

## 5. `break` and `continue`
```java
for (int i = 1; i <= 10; i++) {
    if (i == 7) {
        break;       // exit the loop entirely
    }
    if (i % 2 == 0) {
        continue;    // skip the rest of this iteration, go to the next
    }
    System.out.println(i);   // prints 1, 3, 5
}
```

## 6. Nested Loops
```java
for (int row = 1; row <= 3; row++) {
    for (int col = 1; col <= 3; col++) {
        System.out.print(row * col + "\t");
    }
    System.out.println();
}
```
The inner loop completes all of its iterations for each single iteration of the outer loop —
useful for grids, tables, and (next week) 2D arrays.

## 7. Common Pitfalls
```java
// Off-by-one: this misses index 0 and goes one past the end at index == size
for (int i = 1; i <= size; i++) { /* ... */ }   // BUG if intended range is [0, size)

// Infinite loop: forgot to update n
int n = 0;
while (n < 5) {
    System.out.println(n);
    // missing n++;  -> loops forever
}
```
Always double-check a loop's boundary conditions (`<` vs. `<=`, starting at `0` vs. `1`) against
what you actually intend to iterate over, and confirm every loop variable is actually updated
somewhere in the body.

## 8. In-Class Exercise
Write a sentinel-controlled loop that reads integers until `-1` is entered and prints the running
total and count after each number (not just at the end), so students can see the accumulation
pattern happening live.
