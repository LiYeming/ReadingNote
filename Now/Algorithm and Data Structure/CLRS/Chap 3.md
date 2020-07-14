# Growth of Functions

Consider large enough input sizes, we are studying the **asymptotic** efficiency of algorithms.

Asymptotically efficiency criteria is a good choice for algorithm selection, except for very small inputs.

This chapter introduced some standard simplification method of asymptotic analysis of algorithms.

## Asymptotic notation

A little abusing $T(n)$, but not misuse it.



**$\Theta-$notation**

$\begin{aligned} \Theta(g(n)) = \{f(n) : &\text{there exist positive constants $c_1$, $c_2$, and $n_0$ such that} \\&0\leq c_1 g(n) \leq f(n) \leq c_2 g(n) \text{ for all } n\geq n_0\}\end{aligned}$

If $f(n) = \Theta(g(n))$, $g(n)$ is an **asymptotically tight bound** for $f(n)$.

Lower-order terms of $f(n)$ can be ignored since they asymptotically go to zero for large enough $n$.



**$O-$notation**

When we have only an asymptotic upper bound, we use **O-notation**.

$\begin{aligned} O(g(n)) = \{f(n) : &\text{there exist positive constant $c$ and $n_0$ such that} \\&0\leq f(n) \leq c g(n) \text{ for all } n\geq n_0\}\end{aligned}$

$\Theta-$ is stronger than $O-$. In the literature, O-notation informally describing asymptotically tight bounds. But a distinction is used here.



Theoretically, $\Theta(n^2)$ bound on the running time of insertion sort on worse scenario, but not every case. $O(n^2)$ is useful here, it only means a worst-cast scenario here.



**$\Omega -$ notation​**

$\Omega-$ notation provides an **asymptotic lower bound**.

$\begin{aligned} \Omega(g(n)) = \{f(n) : &\text{there exist positive constant $c$ and $n_0$ such that} \\&0\leq  c g(n)\leq f(n) \text{ for all } n\geq n_0\}\end{aligned}$



**Theorem:** $\Theta -$ is equivalent to $O-$ and $\Omega-$ combined.



**$o-$notation**

$o-$notation denote an upper bound(not necessary tight) that is not asymptotically tight.

$\begin{aligned} o(g(n)) = \{f(n) : &\text{for any positive constant c > 0,} \\ &\text{there exist positive constant $n_0$ such that} \\ &0\leq f(n) < c g(n) \text{ for all } n\geq n_0\}\end{aligned}$



The main difference between $O-$ and $o-$ notation is $\exist c$ in $O-$, and  $\forall c$ in $o-$. Intuitively, $\lim_{n\to \infty}\frac{f(n)}{g(n)} = 0$ if $f(n) = o(g(n))$.



**$\omega-$notation**

$f(n) \in \omega(g(n))$ if and only if $g(n) \in o(f(n))$.

Intuitively, $\lim_{n\to \infty}\frac{f(n)}{g(n)} = \infty$ if $f(n) = \omega(g(n))$.