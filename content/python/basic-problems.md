# [Logic Building Problems](https://www.geeksforgeeks.org/dsa/logic-building-problems/)

## Sum of integers

1 + 2 + ... + n

(n * (n+1)) / 2

## Sum of squares

1^2 + 2^2 + ... + n^2 

(n * (n + 1) * (2 * n + 1)) / 6

Prevent overflowing: (n * (n + 1) // 2) * (2 * n + 1) // 3

## Sum of Arithmetic Series

d = a2 - a1

sum = a1 + (n - 1) * d

# Sorting

## Quick sort

1. Choose a Pivot: Select an element from the array as the pivot. The choice of pivot can vary (e.g., first element, last element, random element, or median).
1. Partition the Array: Rearrange the array around the pivot. After partitioning, all elements smaller than the pivot will be on its left, and all elements greater than the pivot will be on its right. The pivot is then in its correct position, and we obtain the index of the pivot.
1. Recursively Call: Recursively apply the same process to the two partitioned sub-arrays (left and right of the pivot).
1. ase Case: The recursion stops when there is only one element left in the sub-array, as a single element is already sorted.
