# Dynamic Programming

When developing a dynamic programming algorithm, we follow a sequence of four steps:

1. Characterize the structure of an optimal solution.
2. Recursively define the value of an optimal solution.
3. Compute the value of an optimal solution, typically in a bottom-up fasion.
4. Construct an optimal solution from computed information.

## Rod cutting

**Problem:** Given a rod of length $n$ inches and a table of prices $p_i$ for $i = 1,2, \dots, n$, determine the maximum revenue $r_n$ obtainable by cutting up the rod and selling the pieces.

$r_n = \max (p_n, r_1 + r_{n-1}, r_2 + r_{n-2}, \dots, r_{n-1} + r_1).$ for $n\geq 1$.

The rod-cutting problem exhibits **optimal substructure**: optimal solutions to a problem incorporate optimal solutions to related subproblems, which we may solve independently.

$r_n = \max_{1\leq i\leq n} (p_i + r_{n-i})$.

Dynamic programming uses additional memory to save computation time:

- time-memory tradeoff
- Two equivalent ways to implement a dynamic programming approach
  - **Top-down with memoization.** Save the result of each subproblem in a container and check make checks to see if whether it has previously solved.
  - **Bottom-up method.** Sort the subproblems by size and sovle them in size order, smallest first.

When we think about a dynamic-programming problem, we should understand the set of subproblems involved and how subproblems depend on one another.



We can extend the dynamic-programming approach to record not only the optimal value but also the optimal choice that led to the optimal value.

- This is easily implemented by specifying "choice" made upon "state" of the problem.

## Matrix-chain multiplication

**matrix-chain multiplication problem**, given a chain $<A_1, A_2, \dots, A_n>$ of $n$ matrices, where for $i = 1,2,\dots, n$, matrix $A_i$ has dimension $p_{i-1} \times p_{i}$, fully parenthesize the product $A_1A_2\dots A_n$ in a way that minimizes the number of scalar multiplications.

1. Structure of an optimal parenthesization
   Consider evaluation of $A_i A_{i+1}\dots A_{j}$, we can split the product between $A_k$ and $A_{k+1}$ for some integer $k$. That is we first compute $A_iA_{i+1}\dots A_k$ and $A_{k+1}\dots A_j$ and finally do the multiplication.
2. A recursive solution
   Let $m[i,j]$ be the minimum number of scalar multiplications needed to compute matrix $A_{i\dots j}$.
   1. $m[i,i] = 0$
   2. $m[i,j] = m[i,k] + m[k+1, j] + p_{i-1}p_kp_{j}.$ if $i < j$.
   3. $s[i,j]$ = the optimal $k$.
   4. 