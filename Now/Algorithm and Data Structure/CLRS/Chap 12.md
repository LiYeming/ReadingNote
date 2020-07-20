# Binary Search Trees

## What is a binary search tree?

Binary search tree is organized in a binary tree. The keys in binary search tree are always stored in such a way as to satisfy the **binary-search-tree property:**

> Let $x$ be a node in a binary search tree. If $y$ is a node in the left subtree of $x$, then $y.key \leq x.key$. If y is a node in the right subtree of $x$, then $y.key \geq x.key$.

The binary-search-tree property allows us to print out all the keys in a binary search tree in sorted order by simple recursive algorithm called **inorder tree walk**.

**Theorem:** If $x$ is the root of an $n$-node subtree, then the call INORDER-TREE-WALK($x$) takes $\Theta(n)$ time.

## Querying a binary search tree

**Theorem:** We can implement the dynamic-set operations SEARCH, MINIMUM, MAXIMUM, SUCCESSOR and PREDECESSOR so that each one runs in $O(h)$ time on a binary search tree of height $h$.

## Insertion and Deletion

Insertion is straight forward. Deletion is more intricate.

Consider deleting $z$.

- If $z$ has no left child, replace $z$ with its right child.
- If $z$ has a left child, but no right child, replace $z$ with its left child.
- If $z$ has both a left child and a right child, find $z$'s successor $y$. We can guarantee that $y$ has no left child.
  - If $y$ is $z$'s right child, then we replace $z$ with $y$.
  - If $y$ is not $z$'s right child, then replace $y$ withs its own right child, and replace $z$ by $y$.

## Randomly built binary search trees

**Theorem:** The expected height of a randomly built binary search tree on $n$ distinct keys is $O(\lg n)$.

