# Greedy Algorithms

A greedy algorithm always makes the choice that looks best at the moment.

- Locally optimality hopefully would lead to global optimality.

## An activity-selection problem

Scheduling several competing activities that require exclusive use of a common resource.

- Dynamic programming

  Each activity is characterized by $[s_i, f_i)$, we wish to select a maximum-size subset of mutually compatible activities.

  $c[i,j] = c[i,k] + c[k,j] + 1$, where $c[i,j]$ is maximum numbers of activities chosen from end of $i$ to begin of $j$.

- Greedy choice

## Elements of the greedy strategy

1. Determine the optimal substructure of the problem
2. Develop a recursive solution
3. Show that if we make the greedy choice, then only one subproblem remains.
4. Prove that it is always safe to make the greedy choice
5. Develop a recursive algorithm that implements the greedy strategy
6. Convert the recursive algorithm to an iterative algorithm

Ingredients:

- **Greedy-choice property:** we can assemble a globally optimal solution by making locally optimal choices.
- **Optimal substructure:**

## Huffman codes





