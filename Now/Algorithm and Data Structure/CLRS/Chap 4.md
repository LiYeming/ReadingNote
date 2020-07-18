# Divide-and-Conquer

Same three steps as before but introduce two more concepts.

- **Recursive case**, when subproblems are large enough to solve recursively.
- **Base case**, subproblems become small enough that we no longer recurse.

Solving recurrences:

- **Substitution Method**, guess and verify.
- **Recursion-Tree Method**, convert the recurrence into a tree whose nodes represent the costs incurred at various levels of the recursion.
- **Master Method**, provides bounds for currences of the form
  $$ T(n) = aT(n/b) + f(n)$$, where $ a > 1$, $b > 1$, and $f(n)$ is a given function.

Technically, often ignore floors, ceilings and boundary conditions, since these would not change the order of growth.

## The maximum-subarray problem

- Brute force solution, try each and every pair of points and find the maximum.

- Consider change of price, find the **maximum subarray**, which is a non-empty contiguous subarray contain values have the largest sum.

  **divide-and-conquer approach**, $A[low\dots high]$  can be divided into $A[low\dots mid]$ and $A[mid + 1\dots high]$. Any contiguous subarray $A[i\dots j]$ must in three cases: 1. in the first part, 2. in the second part, 3. cross the mid point.
  
  Use recursion for the first two cases, as for the third case, we can further divide the problem into two maximum subarray which head or tail stick to the midpoint, which greatly simplifies the problem.
  
  Analyze: $ T(n) = 2T(n/2) + \Theta(n) $.

## Strassen's algorithm for matrix multiplication

- $\Theta(n^3)$ time for ordinary multiplication, Strassen's algorithm runs in $\Theta(n^{\lg 7})$.
- Strassen's method is basically a divide and conquer approach, but it makes the recursion tree slightly less bushy by reducing eight recursive multiplication to seven.

## The Substitution method for solving recurrences

The **substitution method** for solving recurrences comprises two steps:

1. Guess the form of solution
2. Use mathematical induction to find the constants and show that the solution works

$T(n) = 2T(n/2) + n$

1. Guess solution is $T(n) = O(n \lg n)$.
2. Assuming that $T(n) \leq cn\lg n$ for some $c$ holds for all $m < n$, Thus:
   $T(n) \leq 2c (n/2)\lg (n/2) + n \leq cn\lg(n/2) + n \leq cn\lg n$
3. Choose $c\geq 1$ would suffice the inequality above.

## The recursion-tree method for solving recurrences

When hard to find a good guess, recursion-tree can come to help.

In a recursion tree, each node represents the cost of a single subproblem.

The following is a recursion tree for $T(n) = 3T(n/4) + cn^2$.

![image-20200717194454375](/home/liyeming/Desktop/ReadingNote/Now/Algorithm and Data Structure/CLRS/image-20200717194454375.png)

We can deduce:
$$
T(n) = \frac{(3/16)^{\log_4 n} - 1}{(3/16) - 1}cn^2 + \Theta(n ^{\log_4 3})
$$
Thus $T(n) = O(n^2)$.

## The master method for solving recurrences

A "cookbook" method for solving recurrences of the form $T(n) = aT(n/b) + f(n)$. 

**The Master Theorem:**

Let $a\geq 1$ and $b \geq 1$ be constants, let $f(n)$ be a function, and let $T(n)$ be defined on the nonnegative integers by the recurrence.

$T(n) = aT(n/b) + f(n)$,

Then $T(n)$ has the following asymptotic bounds:

1. If $f(n) = O(n^{\log_b a-\epsilon})$ for some constant $\epsilon > 0$, then $T(n) = \Theta(n^{\log_b a})$.
2. If $f(n) = \Theta(n^{\log_b a})$, then $T(n) = \Theta(n^{\log_b a} \lg n)$.
3. If $f(n) = \Omega(n^{\log_b a+\epsilon})$ for some constant $\epsilon > 0$, and if $af(n/b) \leq cf(n)$ for some constant c < 1 and all sufficient large n, then $T(n) = \Theta(f(n)).$

Intuition, compare $f(n)$ and $n^{\log_b a}$, the larger of these two functions determines the solution to the recurrence.

$T(n)  = 2T(n/2) + n\lg n$ falls into the gap between case 2 and case 3.

