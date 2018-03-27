## Algorithms

  - notes from the book "Grokking Algorithms"

### Chapter 1

Logs are flip of exponentials

10^2 <-> log10(100) = 2
10^3 <-> log10(1000) = 3
2^3 <-> log2(8) = 3

Binary search works only on sorted lists, it runs in logarithmic time.

Stupid search (checking each element) will take O(n), binary search take O(log n).

Big O notation tells you how fast an algorithm is.

Algorithms can grow at different rates.

Stupid search has O(n) and binary search has O(log n).

Some common Big O runtimes:
* O(log n) log time - Binary search
* O(n) linear time - Simple search
* O(n * log n) fast sorting algorithm - Quick Sort
* O(n^2) - slow sorting algorithm - Selection Sort
* O(n!) - very slow algorithm - Traveling Salesperson

### Chapter 2

Linked list vs Arrays (vectors)<br>
Linked lists are great if we are going to read all items and inserts and deletes<br>
Arrays are great to read random elements

Run time for common operations

```
_________ |Arrays | Lists
reading   | O(1)   | O(n)
-----------------------------
insertion | O(n)   | O(1)
----------|--------|---------
deletion  | O(n)   | O(1)
```

Selection Sort

Takes O(n*n) time

### Chapter 4 - Quick Sort

A D&C (Divide and Conquer algorithm)

Steps:
1. Figure out the base case - this should be the simplest possible case
2. Divide or decrease the problem until it becomes the base case

Partitioning:
* Pick an element in the array, that's the pivot.
* Find elements larger than the pivot
* Find elements smaller than the pivot

Now you have:
* sub-array that's less thant the pivot
* the pivot
* sub-array of all the numbers greater than the pivot

The two sub arrays are not sorted, they're just partitioned.

```python
quicksort(lowerValues) + pivot + quicksort(higherValues)
```

Quicksort in Haskell
```haskell
quicksort :: (Ord a) => [a] -> [a]
quicksort [] = []
quicksort (x:xs) =
    let lowersorted = quicksort [l | l <- xs, l <= x]
        highersorted = quicksort [m | m <- xs, m > x ]
    in lowersorted ++ [x] ++ highersorted
```

Quicksort in Python
```python
def quicksort(array):
  if len(array) < 2:
    return array
  else:
    pivot = array[0]
    less = [i for i in array[1:] if i <= pivot]
    greater = [i for i in array[1:] if i > pivot]

    return quicksort(less) + [pivot] + quicksort(greater)
```

Inductive proof:
* One way to prove that an algorithm works.
* Each inductive proof has 2 steps: 1. base case and 2. inductive case

Big O notation revisited

```
______________ | Big O      |
binary search  | O(log n)   |
---------------|--------- --|--
simple search  | O(n)       |
---------------|------------|--
quick sort     | O(n log n) |
---------------|------------|--
selection sort | O(n^2)     |
---------------|------------|--
travelling     | O(n!)      |
  salesman     |            |
```


