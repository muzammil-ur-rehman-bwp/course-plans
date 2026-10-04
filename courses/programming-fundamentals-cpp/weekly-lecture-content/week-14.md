# Week 14 — Lecture Content: Sorting & Searching Algorithms

## 1. Linear Search
```cpp
int linearSearch(const int values[], int size, int target) {
    for (int i = 0; i < size; ++i) {
        if (values[i] == target) {
            return i;   // found at index i
        }
    }
    return -1;          // not found
}
```
Checks each element in order; works on any array, sorted or not, but in the worst case examines
every element.

## 2. Bubble Sort
Repeatedly compares adjacent elements and swaps them if out of order, "bubbling" the largest
unsorted value to the end each pass:
```cpp
void bubbleSort(int values[], int size) {
    for (int pass = 0; pass < size - 1; ++pass) {
        for (int i = 0; i < size - 1 - pass; ++i) {
            if (values[i] > values[i + 1]) {
                int temp = values[i];
                values[i] = values[i + 1];
                values[i + 1] = temp;
            }
        }
    }
}
```
Trace on `[5, 2, 8, 1]`: pass 1 → `[2, 5, 1, 8]`; pass 2 → `[2, 1, 5, 8]`; pass 3 → `[1, 2, 5, 8]`
(sorted). Simple to understand, but makes many comparisons/swaps — adequate for small arrays, not
for large ones.

## 3. Selection Sort
Repeatedly finds the minimum of the unsorted remainder and swaps it into place:
```cpp
void selectionSort(int values[], int size) {
    for (int i = 0; i < size - 1; ++i) {
        int minIndex = i;
        for (int j = i + 1; j < size; ++j) {
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
Trace on `[5, 2, 8, 1]`: find min of whole array (1 at index 3) → swap with index 0 →
`[1, 2, 8, 5]`; find min of `[2, 8, 5]` (2, already in place) → `[1, 2, 8, 5]`; find min of
`[8, 5]` (5 at index 3) → swap → `[1, 2, 5, 8]` (sorted). Selection sort performs fewer swaps than
bubble sort (at most one swap per pass), though both examine a similar number of comparisons.

## 4. Binary Search
Requires a **sorted** array. Repeatedly checks the middle element and discards the half of the
array that cannot contain the target:
```cpp
int binarySearch(const int values[], int size, int target) {
    int low = 0, high = size - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;   // avoids overflow vs. (low + high) / 2
        if (values[mid] == target) {
            return mid;
        } else if (values[mid] < target) {
            low = mid + 1;    // target must be in the right half
        } else {
            high = mid - 1;   // target must be in the left half
        }
    }
    return -1;   // not found
}
```
Trace searching for `5` in `[1, 2, 5, 8]`: `low=0, high=3, mid=1` → `values[1]=2 < 5` → `low=2`;
`low=2, high=3, mid=2` → `values[2]=5 == 5` → found at index 2. Each step eliminates half the
remaining candidates, far fewer comparisons than linear search on a large sorted array — but
binary search **only works on sorted data**; running it on an unsorted array gives wrong answers
without any error.

## 5. Informal Efficiency Intuition
| Algorithm | Requires sorted input? | Rough comparisons (array of size n) |
|---|---|---|
| Linear search | No | up to n |
| Binary search | Yes | roughly log₂(n) |
| Bubble sort | No (sorts) | roughly n² |
| Selection sort | No (sorts) | roughly n² |

This course does not derive Big-O formally (that belongs to a Data Structures & Algorithms
course), but the intuition — binary search "halves the problem" each step, while the sorts
"compare every pair" — is worth internalizing now.

## 6. In-Class Exercise
Implement all four functions, generate a random array of 10 integers, sort it with selection
sort, print it, then search for a present and an absent value with binary search, printing the
result of each.
