# Quicksort

Quicksort is often the best practical choice for sorting because it is remarkably efficient on average.

## Description of quicksort

Process:

- **Divide**, partition the array into two subarraies, such that the first part $A[p...q-1]$ is less than or equal to $A[q]$, which is in turn less than or equal to each element of $A[q+1...r]$.
- **Conquer:** sort the two subarraies by recursive calls to quicksort.
- **Combine:** no need to combine.

## Performance of quicksort

The running time of quicksort depends on whether the partitioning is balanced or unbalanced.

- Balanced, asymptotically equivalent with merge-sort.
- Unbalanced, asymptotically equivalent with insertion-sort.

**Worst case:** $T(n) = T(n - 1) + \Theta(n) = O(n^2)$

**Best case:** $T(n) = 2T(n/2) + \Theta(n) = O(n\lg n)$

**Average case:** much closer to best case than the worst case.

- The behavior of quicksort depends on the relative ordering of the values in the array.
- In the average case, partition produces a mix of good and bad partition, which randomly throughout the tree.

## A randomized version of quicksort

First randomly select an element in $[p\dots r]$, and swap it with $A[r]$. Continue with algorithm above.

## Analysis of quicksort

### Worst-case analysis

$T(n) = \max_{0\leq q \leq n-1} (T(q) + T(n - q - 1)) + \Theta(n),$ which is the worst case time for the procedure QUICKSORT.

Assume $T(n) \leq cn^2$ for some constant $c$. $T(n) = c\cdot \max_{0\leq q \leq n-1} (q^2 + (n - q - 1)^2) + \Theta(n).$

$T(n) \leq c (n-1)^2 + \Theta(n) \leq cn^2 + \Theta(n)$. for some $c$ such that $c(2n - 1)$ term dominates the $\Theta(n)$ term.

### Expected running time

Intuition: $\Theta(\lg n)$ level in the recursion three,  and $O(n)$ work is performed at each level.



Running time, observations

- Each partition fix an array member, thus there would be at most n partitions in a quicksort algorithm.

- Total time spent in the for loop is bounded by the total number of comparison.

  As a matter of fact, the running time of QUICKSORT is O(n + number of comparisons performed).

- Overall bound on the total number of comparison.

  - Each pair of elements is compared at most once, selected pivot would not be selected again.
  - Let $X_{i,j} = I(z_i\ is\ compared\ to\ z_j)$, expected number of comparisons is $\Sigma X_{i,j}$
  - Noticed that when a pivot $x$ is chosen with $z_i < x < z_j$, $z_i$ and $z_j$ are not compared. and a comparison happen when $x == z_i$ or $x == z_j$.
  - Thus $X_{i,j} = \frac{2}{j-i+1}$.
  - Thus $E[X] = \sum_{i = 1}^{n - 1} \sum_{j = i + 1}^{n}\frac{2}{j-i+1} = \sum_{i = 1}^{n - 1} \sum_{k = 1}^{n - i} \frac{2}{k + 1} < \sum_{i=1}^{n-1}\sum_{k=1}^{n}\frac{2}{k} = O(n\lg n).$