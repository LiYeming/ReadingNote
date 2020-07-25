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
   2. $m[i,j] = m[i,k] + m[k+1, j] + p_{i-1}p_kp_{j}.$ if $i < j$. for some $k$.
   3. $s[i,j]$ = the optimal $k$.
3. Computing the optimal costs.
4. Constructing an optimal solution.

## Elements of dynamic programming

Two key ingredients that an optimization problem must have in order for dynamic programming.

- **Optimal substructure**
  An optimal solution to the problem contains within it optimal solutions to subproblems.

  1. A solution to the problem consists of making a choice, making this choice leaves one or more subproblems to be solved.
  2. The choice is made to give an optimal solution.
  3. Given the choice, determine which subproblems ensue and how to best characterize the resulting space of subproblems.
  4. Solutions to the subproblems used within an optimal solution to the problem must themselves be optimal.

  Rule of thumb, try to keep the space as simple as possible and then expand it as necessary.

  Optimal substructure varies across problem domains in two ways:
  
  - How many subproblems on optimal solution to the original problem uses
  - How many choices we have in determining which subproblem to use in an optimal solution
  
  Informally, the running time of dynamic-programming algorithm depends on the product of two factors:
  
  - Number of subproblems
  - How many choices we look at for each subproblem
  
  Subproblems should be independent with each other, that is, solving a subproblem should not affect the solution of another subproblem.
  
- **Overlapping subproblems**

  The total number of distinct subproblems should be small. When a recursive algorithm revisits the same problem repeatedly, we say the optimization problem has **overlapping subproblems.**

  

In general , the bottom-up dynamic-programming algorithm usually outperforms the corresponding top-down memoized algorithm by a constant factor.

- No overhead for recursion and less overhead for maintaining the table.
- Solving only the subproblems that are definitely required.



## Longest common subsequence

Two given sequences $X = <x_1, x_2,\dots, x_m>$, $Y = <y_1, y_2, \dots,y_k>$,  $Z$ is a longest common subsequence of $X$ and $Y$ if $Z$ is a subsequence of both $X$ and $Y$. find $Z$.

1. **Characterizing a longest common subsequence**
   **Theorem** Given $X = <x_1, x_2,\dots, x_m>$, $Y = <y_1, y_2, \dots,y_k>$,  $Z = <z_1, z_2, \dots, z_k>$ is any longest common subsequence of $X$ and $Y$. 
   1. if $x_m = y_k$, $Z_{k-1}$ is an LCS of $X_{m-1}$ an $Y_{k-1}$.
   2. if $x_m \neq y_k$, $z_k \neq x_m$ implies $Z$ is an LCS of $X_{m-1}$ and $Y$.
   3. if $x_m \neq y_k$, $z_k \neq y_n$ implies $Z$ is an LCS of $X$ and $Y_{k-1}$.
2. **A recursive solution**
3. **Constructing an LCS**
4. **Improving the code on time or space it uses**

## Optimal binary search trees

Given a sequence $K = <k_1, k_2, \dots, k_n>$ of $n$ distinct keys in sorted order. We wish to build a binary search tree from these keys. For each key $k_i$, we have a probability $p_i$ that a search will be $k_i$.

Optimal substructure, if an optimal binary search tree $T$ has a subtree $T'$ containing keys $k_i, \dots, k_j$, then this subtree $T'$ must be optimal as well for the subproblem with keys $k_i,\dots, k_j$.

