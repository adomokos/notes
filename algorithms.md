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
---------------|------------|--
simple search  | O(n)       |
---------------|------------|--
quick sort     | O(n log n) |
---------------|------------|--
selection sort | O(n^2)     |
---------------|------------|--
travelling     | O(n!)      |
  salesman     |            |
```

### Chapter 5 - Hash Tables


```
         Hash     Hash
         Tables   Tables            Linked
_______| Avg    | Worst  | Arrays | Lists |
search | O(1)   | O()    | O(1)   | O(n)  |
-------|------- |--------|--------|-------|--
insert | O(1)   | O(n)   | O(n)   | O(1)  |
-------|--------|--------|--------|-------|--
delete | O(1)   | O(n)     O(n)   | O(1)  |
```

Load factos = number of items in hash tables / total number of slots
5 element array with 2 used slots is 2/5

Hash tables:
* have fast search, insert, delete
* are good for modeling relationships from one item to another
* Once the load factor is > 0.7, time to resize the hash table

### Chapter 6 - Breadth-first Search

Shortest path problem - solved with breadth-first algorithm

Graph, models a set of connections.

```python
from collections import deque

graph = {}
graph["you"] = ["alice","bob","claire"]
graph["bob"] = ["anuj", "peggy"]
graph["alice"] = ["peggy"]
graph["claire"] = ["thom", "jonny"]
graph["anuj"] = []
graph["peggy"] = []
graph["thon"] = []
graph["jonny"] = []

def person_is_seller(name):
  return name[-1] == 'm'

def search(name):
  search_queue = deque()
  search_queue += graph[name]
  searched = []
  while search_queue:
    person = search_queue.popleft()
    if not person in searched:
      if person_is_seller(person):
        print person + " is a mango seller!"
        return True
      else:
        search_queue += graph[person]
        searched.append(person)
  return False

```

Running time is at least O(number of edges)
Adding a person takes constant time O(1).
It takes O(number of vertices + number of edges).

### Dijkstra's Algorithm

Finds not the shortest, but the "fastest" path between 2 points.

It has four steps:
1. Find the cheapest node, this is the node you can get to in the least amount of time.
2. Check whether there's a cheaper path to the neighbors of this node. If so, update their costs.
3. Repeat until you've done this for every node in the graph.
4. Calculate the final path.

Terminology:
Each edge in the graph has a number associated to it, these are called weights.
It uses weighted graph.
To calculate shortest path in unweighted graph, use breadth-first search, in a weighted graph, use Dijkstra's algorithm.
Graphs can also have cycles.
Only works with directed acyclic graphs.

