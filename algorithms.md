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


