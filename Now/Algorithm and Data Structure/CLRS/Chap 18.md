# B-Trees

## Definition of B-Trees

A B-tree T is a rooted tree having the following properties:

1. Every node $x$ has the following attributes:

   1. $x.n$, the number of keys current stored in node $x$.
   2. The $x.n$ keys themselves, $x.key_1, x.key_2,\dots , x.key_{x.n}$, stored in nondecreasing order.
   3. $x.leaf$, a boolean value that is true if $x$ is a leaf and false if $x$ is an internal node.

2. Each internal node $x$ also contains $x.n+1$ pointers $x.c_1, x.c_2\dots, x.c_{x.n+1}$ to its children. Leaf nodes have no children, and so their $c_i$ attributes are undefined.

3. The keys $x.key_i$ separate the ranges of keys stored in each subtree: if $k_i$ is any key stored in the subtree with root $x.c_i$, then

   $k_1 \leq x.key_1\leq k_2 \leq x.key_2 \leq \cdots \leq x.key_{x.n} \leq k_{x.n+1}$

4. All leaves have the same depth which is the tree's height $h$.

5. Nodes have lower and upper bound on the number of keys they can contain. We express these bounds in terms of a fixed integer $t \geq 2$ called the minimum degree of B-tree.

