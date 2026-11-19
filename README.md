# Sorting Algorithm Performance Analysis Using Merge Sort and Quick Sort in C

## Project Overview

This project implements two popular sorting algorithms, **Merge Sort** and **Quick Sort**, using the C programming language. It sorts integer records in ascending order, counts the number of element comparisons performed by each algorithm, and compares their performance.

The program also incorporates a new batch of already-sorted records into the existing sorted dataset using an efficient merging technique without sorting the entire collection again.

## Table of Contents

1. [Objectives](#1-objectives)
2. [Problem Analysis](#2-problem-analysis)
3. [Solution Approach](#3-solution-approach)
4. [Implementation](#4-implementation)
5. [Testing](#5-testing)
6. [Complexity Analysis](#6-complexity-analysis)
7. [Discussion](#7-discussion)
8. [Conclusion](#8-conclusion)

---

## 1. Objectives

1. To implement Merge Sort and Quick Sort algorithms in C.
2. To sort a collection of integer records in ascending order.
3. To count and compare the number of element comparisons performed by both algorithms.
4. To analyze sorting performance for ascending, descending, and duplicate-heavy data.
5. To merge a batch of already-sorted records with the existing sorted array efficiently.
6. To evaluate the time complexity and efficiency of different sorting techniques.

## 2. Problem Analysis

The program receives two integers, N and M, representing the sizes of the original and new datasets. It reads N unsorted integers and M pre-sorted integers from an input file.

The original dataset is sorted in ascending order using Merge Sort and Quick Sort separately. Merge Sort counts element comparisons during the merging phase, while Quick Sort uses the last element as the pivot and counts comparisons during partitioning.

The program compares the number of comparisons performed by both algorithms. Finally, it merges the newly sorted batch with the existing sorted dataset without sorting the entire collection again.

## 3. Solution Approach

1. Read the values of N and M and read both datasets from the input file.
2. Create two copies of the original array to run Merge Sort and Quick Sort separately.
3. Apply Merge Sort using the divide-and-conquer technique and count comparisons while merging.
4. Apply Quick Sort using the last element as the pivot and count comparisons during partitioning.
5. Display both sorted arrays and their respective comparison counts.
6. Use the two-pointer merging technique to combine the existing sorted array with the new sorted batch.
7. Display the updated array in ascending order without performing another sorting operation.

## 4. Implementation

### Programming Language
- C

### Source Code

```c
#include <stdio.h>
#include <stdlib.h>

int mergeCount = 0;
int quickCount = 0;

void merge(int a[], int low, int mid, int high) {
    int i = low, j = mid + 1, k = 0;
    int temp[10000];

    while (i <= mid && j <= high) {
        mergeCount++;

        if (a[i] <= a[j])
            temp[k++] = a[i++];
        else
            temp[k++] = a[j++];
    }

    while (i <= mid)
        temp[k++] = a[i++];

    while (j <= high)
        temp[k++] = a[j++];

    for (i = 0; i < k; i++)
        a[low + i] = temp[i];
}

void mergeSort(int a[], int low, int high) {
    if (low < high) {
        int mid = (low + high) / 2;

        mergeSort(a, low, mid);
        mergeSort(a, mid + 1, high);
        merge(a, low, mid, high);
    }
}

int partition(int a[], int low, int high) {
    int pivot = a[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
        quickCount++;

        if (a[j] <= pivot) {
            i++;

            int t = a[i];
            a[i] = a[j];
            a[j] = t;
        }
    }

    int t = a[i + 1];
    a[i + 1] = a[high];
    a[high] = t;

    return i + 1;
}

void quickSort(int a[], int low, int high) {
    if (low < high) {
        int p = partition(a, low, high);

        quickSort(a, low, p - 1);
        quickSort(a, p + 1, high);
    }
}

void display(int a[], int n) {
    for (int i = 0; i < n; i++) {
        printf("%d", a[i]);

        if (i < n - 1)
            printf(" ");
    }

    printf("\n");
}

void updateArray(int a[], int n, int b[], int m) {
    int i = 0, j = 0, k = 0;
    int result[20000];

    while (i < n && j < m) {
        if (a[i] <= b[j])
            result[k++] = a[i++];
        else
            result[k++] = b[j++];
    }

    while (i < n)
        result[k++] = a[i++];

    while (j < m)
        result[k++] = b[j++];

    printf("Updated Array: ");
    display(result, k);
}

int main(int argc, char *argv[]) {
    FILE *fp = stdin;

    if (argc > 1) {
        fp = fopen(argv[1], "r");

        if (fp == NULL) {
            printf("File opening error\n");
            return 1;
        }
    }

    int N, M;

    if (fscanf(fp, "%d %d", &N, &M) != 2 ||
        N < 0 || M < 0 || N > 10000 || M > 10000) {
        printf("Invalid input\n");

        if (fp != stdin)
            fclose(fp);

        return 1;
    }

    int a[10000], b[10000], c[10000];

    for (int i = 0; i < N; i++) {
        if (fscanf(fp, "%d", &a[i]) != 1) {
            printf("Invalid input\n");

            if (fp != stdin)
                fclose(fp);

            return 1;
        }

        b[i] = a[i];
    }

    for (int i = 0; i < M; i++) {
        if (fscanf(fp, "%d", &c[i]) != 1) {
            printf("Invalid input\n");

            if (fp != stdin)
                fclose(fp);

            return 1;
        }
    }

    if (fp != stdin)
        fclose(fp);

    mergeSort(a, 0, N - 1);

    printf("Merge Sort: ");
    display(a, N);

    quickSort(b, 0, N - 1);

    printf("Quick Sort: ");
    display(b, N);

    printf("Merge Comparisons: %d\n", mergeCount);
    printf("Quick Comparisons: %d\n", quickCount);

    updateArray(a, N, c, M);

    return 0;
}
```

## 5. Testing

The program was tested using two different input cases to observe the performance of Merge Sort and Quick Sort.

### Case #1: Random Input

**Input:**

```text
8 4
7 2 9 1 5 3 4 6
0 2 8 10
```

**Output:**

```text
Merge Sort: 1 2 3 4 5 6 7 9
Quick Sort: 1 2 3 4 5 6 7 9
Merge Comparisons: 16
Quick Comparisons: 15
Updated Array: 0 1 2 2 3 4 5 6 7 8 9 10
```

### Case #2: Ascending Input

**Input:**

```text
8 3
1 2 3 4 5 6 7 8
2 4 10
```

**Output:**

```text
Merge Sort: 1 2 3 4 5 6 7 8
Quick Sort: 1 2 3 4 5 6 7 8
Merge Comparisons: 12
Quick Comparisons: 28
Updated Array: 1 2 2 3 4 4 5 6 7 8 10
```

### Testing Summary

| Test Case | Merge Comparisons | Quick Comparisons |
|---|---:|---:|
| Case #1 | 16 | 15 |
| Case #2 | 12 | 28 |

## 6. Complexity Analysis

| Algorithm / Operation | Time Complexity |
|---|---|
| Merge Sort (Best Case) | O(N log N) |
| Merge Sort (Average Case) | O(N log N) |
| Merge Sort (Worst Case) | O(N log N) |
| Quick Sort (Best Case) | O(N log N) |
| Quick Sort (Average Case) | O(N log N) |
| Quick Sort (Worst Case) | O(N²) |
| Array Merging | O(N + M) |

### Space Complexity

- **Merge Sort:** O(N) additional space is required for temporary storage.
- **Quick Sort:** O(log N) average recursion stack space and O(N) worst-case recursion stack space may be required.
- **Array Merging:** O(N + M) additional space is required for the result array.

## 7. Discussion

The program successfully implements Merge Sort and Quick Sort to sort integer records in ascending order. Both algorithms produce the same sorted output, but their element comparison counts differ.

Merge Sort provides consistent performance, while Quick Sort's performance depends on the input order and pivot selection. In Case #1, Quick Sort requires 15 comparisons, whereas Merge Sort requires 16 comparisons. In Case #2, Merge Sort requires 12 comparisons, whereas Quick Sort requires 28 comparisons.

The new sorted batch is merged with the existing sorted array using the merging technique instead of sorting the entire dataset again. The results demonstrate how different sorting algorithms behave under different input conditions and how efficient merging can save processing time.

## 8. Conclusion

The program successfully implements Merge Sort and Quick Sort to sort integer records in ascending order and compare their performance based on element comparisons. The results show that Merge Sort provides consistent performance, while Quick Sort may require different numbers of comparisons depending on the input data. The program also merges a new sorted batch with the existing sorted array efficiently without sorting the entire collection again.

Overall, this experiment helps us understand sorting algorithms, comparison counting, time complexity, and efficient data merging.

---

## How to Run the Program

### Using GCC

Compile the source code:

```bash
gcc main.c -o main
```

Run the program:

```bash
./main
```

Then enter the input manually.

### Using an Input File

Save the input in a file named `input.txt` and run:

```bash
./main input.txt
```

### Online Compiler

You can also execute the program using [Online GDB – C Compiler](https://www.onlinegdb.com/online_c_compiler).

---

**Project:** Sorting Algorithm Performance Analysis

**Language:** C

**Algorithms:** Merge Sort and Quick Sort

**Main Concepts:** Sorting, Comparison Counting, Divide and Conquer, and Sorted Array Merging.
