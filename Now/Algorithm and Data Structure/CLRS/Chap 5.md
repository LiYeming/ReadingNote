# Probabilistic Analysis and Randomized Algorithms

## The hiring problem

Hiring consecutive applicants to find a replacement for incumbent with a cost. Interviewing has a low cost $c_i$, hiring is more expensive $c_h$.



**Probabilistic analysis** is the use of probability in the analysis of problems. 

- Know the distribution of the inputs.
- Analyze our algorithm computing an **average-case running time**.



Ranking applicants in an order is equivalent to assume every permutation of list $<1,2,\dots, n>$ is equally possible to happen.



Generally, an algorithm **randomized** if its behavior is determined not only by its input but also by values produced by a **random-number generator**.

## Indicator random variables

**Indicator random variable** $I_A$ = 1 if A occurs, and 0 if A does not occur

For hiring problem, each and every applicant have probability $p = 1 / i$ to be hired, where is the number of applicant reviewed.

Thus the expected cost for interviewing is thus $O(\lg n)= \frac{1}{2} + \frac{1}{3} + \cdots + \frac{1}{n} + O(1)$.

##　Randomized Algorithms

- **Probabilistic analysis** has an assumption on input distribution.
- **Randomized algorithms** randomizing the input in the algorithm.

How to produce a uniform random permutation of array $[1\dots n]$?

- **Priority method:** First random generate a Priority, and sort according to the Priority.
- **Knuth-Permutation method:** swap the i-th element with randomized chosen element from $A[i]$ to $A[n]$. Repeat from i = 1 to $n$.