# Augmenting Data Structures

In practice, some textbook data structure augmented by storing additional information would suffice.



## Dynamic order statistics

Modify red-black trees so that we can determine any order statistic for a dynamic set in $O(\lg n)$ time.

Augmenting each node from R-B tree with a count $s$ for all elements in the  subtree. the root node is the $s_{left} + 1$ statistic in $s_{root}$ numbers.

SELECT action is easy now, very similar to a divide and conquer.

Rank which return $x$'s rank can be done recursively backward.

Insertion has two phase, first from top to down, find the insert location, second from down to top, resolving all violations to red-black properties.

We only need to add size for all nodes traversed in the first phase. In the second phase, lower.size = std::exchange(upper.size(), upper.size() - lower.size());