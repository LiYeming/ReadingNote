# Getting Started

Framework we use throughout the book to think about the design and analysis of algorithms.

## Insertion Sort

**Sorting Problem:**

**Input:** A sequence of $n$ numbers $<a_1, a_2, \dots, a_n>$.

**Output:** A permutation (reordering) $<a'_1, a'_2, \cdots, a'_n>$ such that $a'_1 \leq a'_2 \leq \cdots \leq a'_n$.

**Insertion Sort**:

```c++
for j = 2 to A.length
    key = A[j];
    i = j - 1;
	while (i > 0) and (A[i] > key)
        A[i + 1] = A[i];
		i = i - 1;
	A[i + 1] = key;
```

**Loop invariants**: At the start of each iteration of the for loop, the subarray A[1...j-1] consists of the elements originally in A[1...j-1], but in sorted order.

We use loop invariants to help us understand why an algorithm is correct. We must show three things about a loop invariant:

- **Initialization**, It is true prior to the first iteration.
- **Maintenance**, If it is true before an iteration, it remains true before the next iteration.
- **Termination**, When the loop terminates, the invariant gives us a useful property that helps show that the algorithm is correct.

## Analyzing algorithms

**Analyzing an algorithm** means predicting the resources that the algorithm requires.

- discard inferior algorithms in the process.

**Assumption of the book**: Single processor, random-access machine.

- Common instructions, arithmetic, control and data movement. and Reasonable assumptions.
- Do not model memory hierarchy.

**Analysis of insertion sort**.

First impression:

- More numbers, more time.
- Already sorted, less time.

Two notions: 

- **Input size**, problem dependent.
  - Also the initial condition matters too.
- **Running time**, the number of primitive operations or "steps" executed

The best case scenario is O(n), the worst case scenario is O(n^2). However, the running time is fixed for a given input.

- `Worse-case running time` of an algorithm is an upper bound on the running time for any input.
- `Average-case running time` of an algorithm.

A more simplifying abstraction, **Rate of growth** of the running time.

- Consider only the leading term of the running time formula.

## Designing algorithms

For insertion sort, we use **incremental** approach, we can also use "divide-and-conquer" approach.

### 2.3.1 The divide-and-conquer approach

- **Divide**, divide the problem into a number of subproblems that are smaller instances of the same problem.
- **Conquer**, solve the subproblems by solving them recursively. If the subproblem sizes are small enough, however, just solve the subproblems in a straight forward manner.
- **Combine**, combine the solutions to the subproblems into the solution for the original problem.

### Analyzing divide-and-conquer algorithms

$$
\begin{align*}
 T(n) =  &O(1) &if\ n\leq c, \\

&aT(n/b) + D(n) + C(n), &otherwise
\end{align*}
$$

$T(n) = O(n \log n)$



