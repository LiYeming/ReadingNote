# Sorting in Linear Time

**Comparison sorts:** sorted order is determined by comparison between the input elements.



## Lower bounds for sorting

Assume an array with distinct members with comparison in the form of $a_i \leq a_j$.

**Decision tree model**: 

- a decision tree is a full binary tree that represents the comparisons between elements that are performed by a particular sorting algorithm operating on an input of a given size.
- In a decision tree, each internal node is annotated by $i:j$ for some $i$ and $j$ in the range $1 \leq i, j \leq n$. 
  - Each internal node indicates a comparison $a_i\leq a_j$, the left tree denotes true case, the right tree denotes the false case.
  - Each leaf node is a permutation of the n elements, and should contain all of them.
  - The execution of a sorting algorithm corresponds to tracing a simple path from the root of the decision tree down to a leaf.



**Theorem:** Any comparison sort algorithm requires $\Omega(n \lg n)$ comparisons in the worst case.

The decision tree should have at least $n!$ leafs, thus the decision tree height $h$ should satisfy: $ h \geq \lg (n!) = \Omega(n \lg n)$. Noticed that the decision tree height is the number of comparisons needed. This proves the theorem.

**Corollary:** Heapsort and merge sort are asymptotically optimal comparison sorts.

## Counting sort

**Counting sort** assumes that each of the n input elements is an integer in the range $0$ to $k$, for some integer $k$. When $k = O(n)$, the sort runs in $\Theta(n)$ time.

- First use a buffer of size $k$, to record the occurrences of each and every element. 
- Then partial sum the buffer to calculate the accurate location each element should locate. 
- Finally, iterate over the original array backwards and use the buffer as a guide to put elements into resulting location. (Reserves stability here.)

In practice, counting sort when we have $k = O(n)$ in which case the running time is $\Theta(n)$.

Counting sort is **stable**: numbers with the same value appear in the output array in the same order as they do in the input array.

## Radix sort

**Radix sort** sort the least significant digit first, then the second-least significant digit and so on. In order for radix sort to work correctly, the digit sorts must be stable.

Assumes each element in the $n$-element array $A$ has $d$ digits, where digit 1 is the lowest-order digit and digit $d$ is the highest-order digit.

```
RADIX_SORT(A, d)
	for i = 1 to d
		use a stable sort to sort array A on digit i.
```

**Lemma:** Given $n$ d-digit numbers in which each digit can take on up to $k$ possible values, Radix-sort correctly sorts these numbers in $\Theta(d(n +k))$ time if the stable sort it uses takes $\Theta(n + k)$ time.

**Lemma:** Given $n$ $b$- bit numbers and any positive integer $r \leq b$, RADIX-SORT correctly sorts these numbers in $\Theta((b/r)(n + 2^r))$ time if the stable sort it uses takes $\Theta(n + k)$ time for inputs in the range $0$ to $k$.

## Bucket sort

Bucket sort assumes that the input is drawn from a uniform distribution and has an average case running time $O(n)$.

Bucket sort divides the interval into $n$ equal-sized **buckets**, then distributes the $n$ input numbers into the buckets. Simply sort the numbers in each bucket and then go through the buckets in order, listing the elements in each.

The running time of bucket sort is $T(n) = \Theta(n) + \sum O(n_i^2)$. Since uniform distribution, thus $T(n) = \Theta(n)+ n\cdot O(2 -1/n) = \Theta(n)$

