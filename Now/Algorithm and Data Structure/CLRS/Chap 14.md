# Augmenting Data Structures

In practice, some textbook data structure augmented by storing additional information would suffice.



## Dynamic order statistics

Modify red-black trees so that we can determine any order statistic for a dynamic set in $O(\lg n)$ time.

Augmenting each node from R-B tree with a count $s$ for all elements in the  subtree. the root node is the $s_{left} + 1$ statistic in $s_{root}$ numbers.

SELECT action is easy now, very similar to a divide and conquer.

Rank which return $x$'s rank can be done recursively backward.

Insertion has two phase, first from top to down, find the insert location, second from down to top, resolving all violations to red-black properties.

We only need to add size for all nodes traversed in the first phase. In the second phase, lower.size = std::exchange(upper.size(), upper.size() - lower.size());

## How to augment a data structure

In general, augmenting a data structure in four steps:

1. Choose an underlying data structure
2. Determine additional information to maintain in the underlying data structure.
3. Verify that we can maintain the additional information for the basic modifying operations on the underlying data structure.
4. Develop new operations.

**Theorem:** Let $f$ be an attribute that augments a red-black-tree $T$ of $n$ nodes, and suppose that the value of $f$ for each node $x$ depends on only the information in nodes $x$, $x.left$, and $x.right$, possibly including $x.left.f$ and $x.right.f$. Then, we can maintain the values of $f$ in all nodes of $T$ during insertion and deletion without asymptotically affecting the $O(\lg n)$ performance of these operations.

## Interval trees

Augment red-black trees to support operations on dynamic sets of intervals. Interval holds interval **trichotomy**, exactly one of these following three holds:

- $i$ and $i'$ overlap.
- $i$ is to the left of $i'$.
- $i$ is to the right of $i'$.

1. **Underlying data structure**, Use lower point of interval to determine the sorted order.
2. **Additional Information,** Each node contains a value $x.max$, which is the maximum value of any interval endpoint stored in the subtree rooted at $x$.
3. **Maintaining the information,** Insertion and deletion takes $O(\lg n)$ time on an interval tree of $n$ nodes. $x. max = \max (x.int.high, x.left.max, x.right.max)$
4. **Developing new operations,** 

