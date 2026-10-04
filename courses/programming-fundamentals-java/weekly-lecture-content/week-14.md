# Week 14 — Lecture Content: Sorting & Searching Algorithms

## 1. Bubble Sort
```java
static void bubbleSort(int[] values) {
    int n = values.length;
    for (int pass = 0; pass < n - 1; pass++) {
        for (int i = 0; i < n - 1 - pass; i++) {
            if (values[i] > values[i + 1]) {
                int temp = values[i];
                values[i] = values[i + 1];
                values[i + 1] = temp;
            }
        }
    }
}
```
Bubble sort repeatedly steps through the array, swapping adjacent elements that are out of order.
Each full pass "bubbles" the largest remaining unsorted element into its correct position at the
end, so each subsequent pass can examine one fewer element (`n - 1 - pass`).

Trace on `{5, 2, 8, 1}`:
```
Pass 1: 5,2,8,1 -> 2,5,8,1 -> 2,5,8,1 -> 2,5,1,8
Pass 2: 2,5,1,8 -> 2,5,1,8 -> 2,1,5,8
Pass 3: 2,1,5,8 -> 1,2,5,8
```

## 2. Selection Sort
```java
static void selectionSort(int[] values) {
    int n = values.length;
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (values[j] < values[minIndex]) {
                minIndex = j;
            }
        }
        int temp = values[i];
        values[i] = values[minIndex];
        values[minIndex] = temp;
    }
}
```
Selection sort finds the minimum of the *unsorted* remainder on each pass and swaps it into place
at the front — unlike bubble sort, each pass performs exactly **one** swap, no matter how
unsorted the data is.

## 3. Linear Search
```java
static int linearSearch(int[] values, int target) {
    for (int i = 0; i < values.length; i++) {
        if (values[i] == target) {
            return i;      // found — return its index
        }
    }
    return -1;             // not found
}
```
Linear search works on any array, sorted or not, but must potentially examine every element.

## 4. Binary Search (Requires a Sorted Array)
```java
static int binarySearch(int[] sortedValues, int target) {
    int low = 0;
    int high = sortedValues.length - 1;

    while (low <= high) {
        int mid = (low + high) / 2;
        if (sortedValues[mid] == target) {
            return mid;
        } else if (sortedValues[mid] < target) {
            low = mid + 1;     // target must be in the right half
        } else {
            high = mid - 1;    // target must be in the left half
        }
    }
    return -1;   // not found
}
```
Binary search repeatedly halves the range it must still search by comparing the target to the
middle element — but this only works because the array is sorted; the comparison at the midpoint
would be meaningless otherwise. Running it on an unsorted array silently produces wrong answers
rather than an obvious error, which is exactly why sortedness is a precondition the caller must
guarantee.

## 5. Informal Efficiency Comparison
| Algorithm | Worst-case comparisons (n elements) | Requires sorted input? |
|---|---|---|
| Bubble sort | ~n²/2 | No |
| Selection sort | ~n²/2 | No |
| Linear search | n | No |
| Binary search | ~log₂(n) | Yes |

For `n = 1000`: bubble/selection sort each do on the order of 500,000 comparisons; linear search
up to 1,000; binary search at most about 10. This is an informal, intuitive introduction to the
efficiency trade-offs; formal Big-O analysis is covered in the Data Structures & Algorithms
course.

## 6. In-Class Exercise
Trace selection sort by hand on `{5, 2, 8, 1, 9}` for the first two passes (write out the array
after each pass). Then binary-search the fully sorted result for `8`, writing down `low`, `high`,
and `mid` at each step.
