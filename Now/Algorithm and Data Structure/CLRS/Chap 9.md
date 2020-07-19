# Medians and Order Statistics

The **$i$th order statistic** of a set of $n$ elements is the $i$th smallest element.

The **median** is the "halfway point" of the set.

- Unique if $n$ is odd.
- Two median can be chosen if $n$ is even. Use lower median this chapter. 



**Selection problem**:

Input: A set $A$ of $n$ distinct numbers and an integer $i$, with $1 \leq i \leq n$.

Output: The element $x \in A$ that is larger than exactly $i-1$ other elements of $A$.



Observation:

- Can be solved $O(n \lg n)$, just repeat heapsort or mergesort $i$ times.



## Minimum and Maximum

- Simple minimum: Go through and compare each and every elements $n-1$ comparisons.
  Optimal since every element should be compared and lose to the winner.

- What if we want to achieve maximum and minimum simultaneously? $T(n) = 2n- 2$ is a possible answer for using algorithm above twice.

  Can be improved by compare two elements at the same time.
  First select two new elements, and compare them. Then compare the larger to the maximum, smaller to the minimum. $T(n) = 3 [n/2]$ this way.

## Selection in expected linear time

Asymptotic running time for general selection problem is $\Theta(n)$.

Algorithm:

	1. Pick a randomized pivot.
 	2. Make a partition. 
 	3. If pivot is $i$-th element. solve the problem. If not, choose one side and repeat the algorithm.

Worst running time is $\Theta(n^2)$.

## Selection in worst-case linear time

A selection algorithm whose running time is $O(n)$ in the worse case.

- Guarantee a good split upon partitioning the input array.

Algorithm:

	1. Divide the $n$ elements of the input array into $[n/5]$ groups of $5$ elements each and at most one group made up of the remaining $n \% 5$ elements.
 	2. Find the median of each bucket by insertion-sorting
 	3. Recursively to find the median $x$ of the $[n/5]$ medians.
 	4. Partition the input array around the median-of-medians $x$ using the modified version of PARTITION. Let $k$ be one more than the number of elements on the low side of partition. so $x$ is the $k$-th smallest.
 	5. If $i = k$ then return $x$. Otherwise, recursively to find the $i$th smallest element on the lowside or high side.

We can see that at least half of the $[n/5]$ group contributes at least 3 elements that are greater than $x$. The number of elements greater than $x$ is at least $3([\frac{1}{2}[\frac{n}{5}]] - 2) \geq \frac{3n}{10} - 6$

$T(n) = \Theta(n)$.



Solving the selection problem by sorting and indexing is asymptotically inefficient.