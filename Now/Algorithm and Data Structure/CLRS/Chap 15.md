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
  - Top-down with memoization. Save the result of each subproblem in a container and check make checks to see if whether it has previously solved.
  - Bottom-up method.