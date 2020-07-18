# Heapsort

Illustrate how to use a data structure to manage information.

## Heaps

Heap data structure is an array object that we can view as a nearly complete binary tree.

There are two kinds of binary heaps: max-heaps and min-heaps. In both kinds, the values in nodes satisfies **heap property**, which depends on the type of heap.

**height** of a node in a heap is the number of edges on the ongest simple downward path from the node to a leaf. The height of a heap is defined as the height of the root node.

## Maintaining the heap property

max_heapify assumes the left subtree and right subtree are heaps, but the root node may violate the heap property.

its implementation is to exchange the value of root and the larger node of the left subnode and right subnode, and recursively call max_heapify to the changed subnode.

running time of max_heapify is $T(n) \leq T(2n/3) + \Theta(1), T(n) = O(\lg(n)) = O(h)$;

## Building a heap

```
A.heap-size = A.length;
for i = A.length / 2 downto 1
	max_heapify(A, i);
```

Invariant: at the start of each iteration of the for loop of lines 2-3, each node i + 1, i + 2, ... n is the root of a max-heap.

$O(Build\_max\_heap) = O(n)$

## Heapsort algorithm

```
Build_max_heap(A);
for i = A.length to 2:
	exchange A[i] with A[1];
	A.heap_size = A.heap_size - 1;
	Max_heapify(A, 1)
```

$O(heapsort) = O(n\lg n)$

## Priority queues

A most popular application of a heap: an efficient priority queue.

A **priority-queue** is a data structure for maintaining a set $S$ of elements, each with associated value called a **key**. A max-priority-queue supports: 

- Insert, insert a new element $x$.
- Maximum, return the maximum element in $S$.
- Extract_max, return and pop the maximum element in $S$.
- Increase_key, increases the value of element $x$'s key to the new value $k$.

We can use a heap to implement priority queue.

