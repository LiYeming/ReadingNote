# Red-Black Trees

## Properties of red-black trees

A **red-black tree** is a `binary search tree` with one extra bit of storage per node: its **color**, which is either RED or BLACK.

A red-black tree ensure that no such path is more than twice as long as any other, so the tree is approximately **balanced**.



Formally, a red-black tree is a binary tree that satisfies the following **red-black properties**.

1. Every node is either black or red.
2. The root is black
3. Every leaf is black
4. If a node is red, then both its children are black
5. For each node, all simple paths from the node to descendant leaves contain the same number of black nodes.

In the remainder of this chapter, we ignore leaves.



We call the number of black nodes on any simple path from, but not including, a node $x$ down to a leaf the **black-height** of the node, denoted $bh(x)$. It is well defined by red-black properties. 

black-height of a tree is defined as black-height of its root.



**Lemma:** A red-black tree with $n$ internal nodes has height at most $2 \lg (n+1)$.

Proof: 

1. Any subtree rooted at any node $x$ contains at least $2^{bh(x)} - 1$ internal nodes.
   Prove by induction, if $h(x) = 0$, $x$ must be a leaf, and the subtree rooted at $x$ indeed contains 0 internal nodes.
   if this is true for $h(x) < n$, we need to show it still holds for $h(x) = n$. Consider the left and right subtree should easily prove the step.
2. According to red-black properties 4, at least half of nodes on the simple path from root to leaf must be black nodes. Thus the black-height of root must be at least $h/2$.
3. Total internal node should have $n \geq 2^{h/2} - 1$, thus $2 \lg (n + 1) \geq h$



## Rotations

Insertion and Delete of binary search tree may violate red-black properties. As a result, we need to change the colors and some pointer structure.

Pointer structures are changed through **rotation**, which is a local operation in a search tree that preserves the `binary-search-tree` property.

## Insertion

1. First insert a node as if it is an ordinary binary search tree. Color the newly inserted node red.
2. Use RB_Insert_Fixup to recolor nodes and perform rotations.

What to fix? 

- Property 2, Root may be red. 
- Property 4, Red newcomer may have red parent.

Cases, assuming we insert node $z$.

1. $z$'s uncle $y$ is red
   $z$ and $z$'s parent is red, since $z$'s parent is red, z's grandparent is black.
   To maintain property 5, let grandparent be red, and color z's parent and y black. Check $z$'s grandparent in the next iteration.

2. $z$'s uncle is black and $z$ is a right child

3. $z$'s uncle is black and $z$ is a left child

   in case 2, if $z$ is a right child, we can left-rotate $z$, and come to case 3.

   in case 3, $z$ is a left child, we can right-rotate $z$'s parent, and do the corresponding coloring to maintain Property 5.

